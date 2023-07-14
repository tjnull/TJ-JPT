# Bash port scanner

```bash
#!/bin/bash
function ctrl_c(){
	echo -e "\n\n[!] Exiting...\n"
	tput cnorm; exit 1
}

# Ctrl+C
trap ctrl_c INT

tput civis

for port in $(seq 1 65535); do
	timeout 1 bash -c "echo '' > /dev/tcp/x.x.x.x/$port" 2>/dev/null && echo "[+] Port $port - Open" &
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

hosts=(172.18.0.1 172.18.0.2 172.19.0.1 172.19.0.2 172.19.0.3)
tput civis; for host in ${hosts[@]}; do
	echo -e "\n[i] Scanning ports for host $host:"
	for port in $(seq 1 65535); do
		timeout 1 bash -c "echo '' > /dev/tcp/$host$/$port" 2>/dev/null && echo -e "\t[+] Port $port - Open" &
	done; wait
done; tput cnorm
```
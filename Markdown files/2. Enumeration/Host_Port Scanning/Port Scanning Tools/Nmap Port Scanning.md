# Nmap Port Scanning

## Nmap: Most common

- `nmap -sC -sV x.x.x.x` - Full TCP connect (`-sC`) scan and service enumeration (`-sV`)
- `nmap -sV -Pn x.x.x.x` - Service enumeration (`-sV`) and skip pinging the host before scanning (`-Pn`)
- `nmap -T4 -sC -sV x.x.x.x` - Setting timing template to aggressive (`-T4`), full TCP connect (`-sC`) scan, and service enumeration (`-sV`)
- `nmap -A x.x.x.x`	- Enable all checks (OS and version detection, script scanning, and traceroute) against an service found (`-A`)

## Deeper dive:

The top ports flag in nmap can be leveraged to quickly get results back. This allows for the testers to start manually looking into the more common ports while more detailed scans take place in the background. Use the following on the active hosts:

```sh
nmap -oN nmap_x.x.x.x-top_ports_1000.txt -sV --top-ports 10000 --open -v -Pn -iL x.x.x.x_Active_IPs.txt 
```

This will do standard service detection (`-sV`), scan and display only the top thousand open ports, be verbose (`-v`), skip pinging the host (`-Pn`), and output results to a text file (`-oN`).

For more verbosity, include more "v's" in the verbose flag, i.e. `-vvv`.

If the network is fast and responsive, the execution time and number of top ports scanned can be increased:

```sh
nmap -oN nmap_x.x.x.x-top_ports_5000.txt -sV --top-ports 5000 --open -v -Pn -T3 -iL x.x.x.x_Active_IPs.txt 
```

Again, the main goal of these initial scans is to get results into testers' hands quickly so manual analysis can begin. If not on a time crunch, you can scan all ports with `-p-` and drop the `--top-ports` flag.

## Other Nmap options: 

NSE scripts are great resources for enumerating more out of a service. 

*Location: /usr/share/nmap/scripts/*

- `nmap --scripts vuln,safe,discovery -oN results.txt x.x.x.x`

You can search the script directory for a specific service:

- `ls -l /usr/share/nmap/scripts/ | grep http`

You can also have nmap scan through a proxy, i.e. this will scan through a Socks proxy: 

- `nmap --proxies socks4://proxy-ip:8080 x.x.x.x`

This supports http and socks4 proxies. 

## Other scanning methods:
 
TCP run all scripts:

```sh
nmap -vv -Pn -A -sC -sS -T 4 -p- x.x.x.x
```

UDP run all scripts:

```sh
nmap -vv -Pn -A -sC -sU -T 4 --top-ports 200 x.x.x.x
```

## Scan Through ssh proxy: (can only do full connect scan -sT)

Setup ssh: 
```sh
ssh -D 1337 <username@ssh_host>
```

```sh
nmap -oN <output_file.txt> --exclude 127.0.0.1 --top-ports 1800 -T4 -vvv --max-rtt-timeout 500ms --max-retries 3 --host-timeout 30m --max-scan-delay 500ms -Pn -sT --version-intensity 1 --open --proxies socks4://127.0.0.1:1337 -iL <IP_List.txt>
```

# Ping Sweep: Nmap method

To perform a ping sweep in nmap, the -sn flag is vital. In its simplest form, any of the following three options could be used for scanning:

```sh
nmap -sn x.x.x.x/24
nmap -sn x.x.x.1-254
nmap -sn x.x.x.*
```

You can also grep out the IPs and cut out fluf:

```sh
nmap -sn 172.x.x.x/24 | grep "172" | cut -f 5 -d ' '
```

A slower, more stealthier approach that utilizes the files containing the IP address split (as seen in the first section above) would be:

```sh
nmap --randomize-hosts -sn -T2 -oN nmap_discoveryScan_x.x.x.x-16.txt -iL x.x.x.x_IP_range.split.txt
```

This will export the results into a text file (`-oN`). Randomized hosts is optional, depending on the customer and the testing situation. The flag, `-oA`, can be used in place of `-oX` or `-oN`, as `-oA` will output the results to all output formats. 

The results for both command options shown above will be the list of hosts that responded to the ping, thus are up and alive.

## Nmap:

- `nmap -sC -sV 172.21.0.0`
- `nmap -Pn -sC -sV -p- 172.21.0.0`
- `nmap -sV -Pn 172.21.0.0`
- `nmap -T4 -sC -sV 172.21.0.0`
- `nmap -A 172.21.0.0`

## Nmap Stealth: 

- `nmap -sS -sC -sV 172.21.0.0`
- `nmap -sS -p- 172.21.0.0`

## UDP Scan: 

- `nmap -sS -sU -Pn -sV 172.21.0.0`
- `nmap -sU -A --top-ports=20 --version-all`
- `nmap -sU -A -p 53,67,68,161,162 --version-all`
- `unicornscan -mU -p ,161,162,137,123,138,1434,445,135,67,68,53,139,500,637,162,69`

## IPv6 Scan:

Nmap Scripts: 

Location: /usr/share/nmap/scripts/

- `nmap --scripts vuln,safe,discovery -oN results.txt target-ip`

Scans through Socks proxy: 

- `nmap --proxies socks4://proxy-ip:8080 target-ip`
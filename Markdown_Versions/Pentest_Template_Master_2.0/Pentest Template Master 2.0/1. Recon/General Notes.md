# "PCAP IT OR IT DIDNT HAPPEN...its up to you if you need to"


## tcpdump:

- tcpdump -i eth0
- tcpdump -c -i eth0
- tcpdump -A -i eth0
- tcpdump -w 0001.pcap -i eth0
- tcpdump -r 0001.pcap
- tcpdump -n -i eth0
- tcpdump -i eth0 port 22
- tcpdump -i eth0 -src 172.21.10.X
- tcpdump -i eth0 -dst 172.21.10.X

Other tools: 

Tshark (Command Line Wireshark)
Wireshark


## Network Scanning

NetDiscover (ARP Scanning):
- netdiscover -i eth0
- netdiscover -r 172.21.10.0/24

Nmap:

- nmap -sn 172.21.10.0/24
- nmap -sn 172.21.10.1-253
- nmap -sn 172.21.10.*

Nbtscan: 
- nbtscan -r 172.21.1.0/24

Linux Ping Sweep (Bash)

- for i in {1..254} ;do (ping -c 1 172.21.10.$i | grep "bytes from" &) ;done

Windows Ping Sweep (Run on Windows System)

- for /L %i in (1,1,255) do @ping -n 1 -w 200 172.21.10.%i > nul && echo 172.21.1.%i is up.



## Host Scanning

Nmap:

- nmap -sC -sV 172.21.0.0
- nmap -Pn -sC -sV -p- 172.21.0.0
- nmap -sV -Pn 172.21.0.0
- nmap -T4 -sC -sV 172.21.0.0
- nmap -A 172.21.0.0

Nmap Stealth: 
- nmap -sS -sC -sV 172.21.0.0
- nmap -sS -p- 172.21.0.0


UDP Scan: 
- nmap -sS -sU -Pn -sV 172.21.0.0
- nmap -sU -A --top-ports=20 --version-all
- nmap -sU -A -p 53,67,68,161,162 --version-all 
- unicornscan -mU -p ,161,162,137,123,138,1434,445,135,67,68,53,139,500,637,162,69


IPv6 Scan:

Nmap Scripts: 

Location: /usr/share/nmap/scripts/

- nmap --scripts vuln,safe,discovery -oN results.txt target-ip

Scans through Socks proxy: 

- nmap --proxies socks4://proxy-ip:8080 target-ip

DNSRecon: 

- dnsrecon -d www.example.com -a 
- dnsrecon -d www.example.com -t axfr
- dnsrecon -d <startIP-endIP>
- dnsrecon -d www.example.com -D <namelist> -t brt

Dig: 

- dig www.example.com + short
- dig www.example.com MX
- dig www.example.com NS
- dig www.example.com> SOA
- dig www.example.com ANY +noall +answer
- dig -x www.example.com
- dig -4 www.example.com (For IPv4)
- dig -6 www.example.com (For IPv6)
- dig www.example.com mx +noall +answer example.com ns +noall +answer
- dig -t AXFR www.example.com

Sublis3r:

- Sublist3r -d www.example.com
- Sublist3r -v -d www.example.com -p 80,443

OWASP AMASS: 

- amass enum -d www.example.com
- amass intel -whois -d www.example.com
- amass intel -active 172.21.0.0-64 -p 80,443,8080,8443
- amass intel -ipv4 -whois -d www.example.com
- amass intel -ipv6 -whois -d www.example.com


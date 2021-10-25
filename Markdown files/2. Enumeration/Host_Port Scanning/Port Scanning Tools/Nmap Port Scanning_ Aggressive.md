# Nmap Port Scanning: Aggressive

After initial results of the top ports scans have been completed and initial investigation into the open ports is about to being, aggressive nmap scans can be initiated in the background. 
```
nmap -oN nmap_x.x.x.x-all_ports_aggressive.txt -T4 -vvv --max-rtt-timeout 300ms --max-retries 3 --host-timeout 30m --max-scan-delay 500ms -Pn -A -p- --version-intensity 1 -iL x.x.x.x_Active_IPs.txt
```
Broken down:
  - `-T4`: timing template - 4 is faster execution
  - `-vvv`: Max verbosity
  - `--max-rtt-timeout`: time for nmap to wait for a probe to respond before giving up (100ms is acceptable for a local network scan)
  - `--max-retries`: number of times for nmap to retry if nmap received no response
  - `--host-timeout`: If host is taking too long to scan, it'll stop scanning it at the set time. (nmap will be scanning other hosts during this time, so it isn't just waiting around for 30 mins)
  - `--max-scan-delay`: specifies the largest delay that Nmap will allow. The lower the number, the more likely of missing ports
  - `-Pn`: no ping
  - `-A`: enables additional advanced and aggressive options. Equivalent of `-O`, `-sV`, `-sC`, and `--traceroute`
  - `-p-`: scan all ports (1 through 65535)
  - `--version-intensity`: specifies which probes should be applied. The higher the number, the more likely it is the service will be correctly identified. However, high intensity scans take longer. The intensity must be between 0 and 9. The default is 7

UDP equivalent:
```
nmap -oX nmap_x.x.x.x-top_ports_1800_UDP_aggressive.xml --top-ports 1800 -sU -T4 -vvv --max-rtt-timeout 300ms --max-retries 3 --host-timeout 30m --max-scan-delay 500ms -Pn -sV --version-intensity 1 -iL x.x.x.x_Active_IPs.txt
```
If scans are not completing or skipping hosts too quickly, change the `--max-rtt-timeout` and `--max-scan-delay` settings. Additionally, for a slower, more complete, stealthier approach, the following can be used:
```
nmap -sT -Pn -p- --max-parallelism 1 --max-retries 0 --max-rtt-timeout 1000ms --max-hostgroup 1 -oX nmap_x.x.x.x-all_ports_slow.xml -iL x.x.x.x_Active_IPs.txt
```
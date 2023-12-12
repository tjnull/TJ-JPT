# Nmap Port Scanning: Slow/Steathly

For slow discovery of hosts within a range:

```sh
nmap -sT -Pn --top-ports 100 --max-rate .33  --max-parallelism 1 --max-retries 0 --max-rtt-timeout 1000ms --max-hostgroup 1 -e eth0:x -oN <output_filename.txt> <subnet>
```

For slow all-ports scanning

```sh
nmap -sT -Pn -p- --max-parallelism 1 --max-retries 0 --max-rtt-timeout 1000ms --max-hostgroup 1 -oN <output_filename.txt> -iL <hostlist>
```
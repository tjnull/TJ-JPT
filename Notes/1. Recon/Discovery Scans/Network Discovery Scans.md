# Network Discovery Scans

## NetDiscover (ARP Scanning):
- `netdiscover -i eth0`
- `netdiscover -r 172.21.10.0/24`

## Dsniff Arpspoof

First enable Linux box to act as a router:
`echo 1 > /proc/sys/net/ipv4/ip_forward`

Then run `arpspoof`:
`arpspoof -i <interface> -t <target> -r <host>`

For example, to intercept traffic between targets, use:
`arpspoof -i eth0 -t 192.168.4.11 -r 192.168.4.16`

## Nmap:

- `nmap -sn 172.21.10.0/24`
- `nmap -sn 172.21.10.1-253`
- `nmap -sn 172.21.10.*``

You can also grep out the IPs and cut out fluf:
```
nmap -sn 172.x.x.x/24 | grep "172" | cut -f 5 -d ' '
```

A slower, more stealthier approach that utilizes the files containing the IP address split (as seen in the first section above) would be:
```
nmap --randomize-hosts -sn -T2 -oN nmap_discoveryScan_x.x.x.x-16.txt -iL x.x.x.x_IP_range.split.txt
```

This will export the results into a text file (`-oN`). Randomized hosts is optional, depending on the customer and the testing situation. The flag, `-oA`, can be used in place of `-oX` or `-oN`, as `-oA` will output the results to all output formats. 

The results for both command options shown above will be the list of hosts that responded to the ping, thus are up and alive.

## Nbtscan: 
- `nbtscan -r 172.21.1.0/24`

## Masscan
- `masscan 172.21.10.0/24 --ping`

## Ping Sweeps

See Ping Sweep directory.
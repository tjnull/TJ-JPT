# DNS Enumeration Tools

### Nmap Enumeration
```
$ ls -lh /usr/share/nmap/scripts/ | grep dns
-rw-r--r-- 1 root root  1499 Oct 12 09:29 broadcast-dns-service-discovery.nse
-rw-r--r-- 1 root root  5329 Oct 12 09:29 dns-blacklist.nse
-rw-r--r-- 1 root root 10100 Oct 12 09:29 dns-brute.nse
-rw-r--r-- 1 root root  6639 Oct 12 09:29 dns-cache-snoop.nse
-rw-r--r-- 1 root root 15152 Oct 12 09:29 dns-check-zone.nse
-rw-r--r-- 1 root root 14826 Oct 12 09:29 dns-client-subnet-scan.nse
-rw-r--r-- 1 root root 10168 Oct 12 09:29 dns-fuzz.nse
-rw-r--r-- 1 root root  3803 Oct 12 09:29 dns-ip6-arpa-scan.nse
-rw-r--r-- 1 root root 12702 Oct 12 09:29 dns-nsec3-enum.nse
-rw-r--r-- 1 root root 10580 Oct 12 09:29 dns-nsec-enum.nse
-rw-r--r-- 1 root root  3441 Oct 12 09:29 dns-nsid.nse
-rw-r--r-- 1 root root  4364 Oct 12 09:29 dns-random-srcport.nse
-rw-r--r-- 1 root root  4363 Oct 12 09:29 dns-random-txid.nse
-rw-r--r-- 1 root root  1456 Oct 12 09:29 dns-recursion.nse
-rw-r--r-- 1 root root  2195 Oct 12 09:29 dns-service-discovery.nse
-rw-r--r-- 1 root root  5679 Oct 12 09:29 dns-srv-enum.nse
-rw-r--r-- 1 root root  5765 Oct 12 09:29 dns-update.nse
-rw-r--r-- 1 root root  2123 Oct 12 09:29 dns-zeustracker.nse
-rw-r--r-- 1 root root 26574 Oct 12 09:29 dns-zone-transfer.nse
-rw-r--r-- 1 root root  3910 Oct 12 09:29 fcrdns.nse
$ nmap x.x.x.x -v -p 53 --script=exampleScript1.nse,exampleScript2.nse
```

### Leaking Hostname
```
$ nslookup
> server <target_IP>
Default server: <target_IP>
Address: <target_IP>#53
> 127.0.0.1
** might give back internal hostname
> <target_IP>
** might give back internal hostname
```

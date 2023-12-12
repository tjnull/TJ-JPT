# DNS Reverse Lookup Brute Force

```
for ip in $(seq 1 255);do host 192.168.31.$ip;done |grep -v "not found"
```

```
netdiscover
```
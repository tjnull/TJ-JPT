# Ad hoc netcat port scanner (no service enumeration) 

In situations when we connect to an internal target host and are unable to use nmap or another port scanner for the internal network, if the host is Linux and if it has netcat installed, it can be leveraged to do port scanning. Note that the example code below is not very fast, but it works in desperate situations.  

Example target is `10.100.0.0/16`
```
#!/bin/bash
for i in {0..255}; do
    for j in {0..255};do
        for k in {0..65535};do
            nc -v -z -n -w 1 10.100.${i}.${j} ${k} >> nc_port_scan.txt
        done
    done
done
```
Source: [https://www.cyberciti.biz/faq/linux-port-scanning/](https://www.cyberciti.biz/faq/linux-port-scanning/)
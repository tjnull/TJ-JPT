# Ping Sweep: Bash Method

Similar to the PowerShell example, using bash is useful if you connect to a \*nix box and only have bash available to you.
```
#!/bin/bash

for ip in $(seq 1 254); do 
ping -c 1 x.x.x.$ip | grep "bytes from" | cut -d " " -f 4 |cut -d ":" -f 1 &
done
```
Or in oneliner format:
```
for i in {1..254} ;do (ping -c 1 x.x.x.$i | grep "bytes from" | cut -d " " -f 4 |cut -d ":" -f 1 &) ;done
```

Alternatively, you can load the IPs to ping from a file:
```
#!/bin/bash

up_ipfile='online_server.txt'

while IFS= read -r ips; do
       ping -c 1 $ips > /dev/null 2>&1
       if [ $? -eq 0 ]; then
             echo $ips >> $up_ipfile
       fi
done < ListOfIPsToPing.txt
```
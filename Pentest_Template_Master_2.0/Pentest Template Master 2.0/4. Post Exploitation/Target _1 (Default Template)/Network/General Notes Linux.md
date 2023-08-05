## What does the targets network look like:

- /sbin/ifconfig -a
- /sbin/ip addr
- cat /etc/network/interfaces
- cat /etc/sysconfig/network
- ip addr show

## Network configuration Settings: 

- cat /etc/resolv.conf
- cat /etc/sysconfig/network
- cat /etc/networks
- iptables -L
- hostname
- dnsdomainname

## List all current connections

- lsof -i
- lsof -i :80
- grep 80 /etc/services
- netstat -antup
- netstat -antpx
- netstat -tulpn
- chkconfig --list
- chkconfig --list | grep 3:on

## Check the routes: 

- arp -e
- route
- route -n 
- /sbin/route -nee
- ip route list

References: 


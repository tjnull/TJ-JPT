# RDP Enumeration Tools

### Nmap Enumeration
```
$ ls -l /usr/share/nmap/scripts/ | grep rdp
```
```
$ nmap x.x.x.x -v -p 3389 --script=exampleScript1.nse,exampleScript2.nse
```

### Manual Connnection
```
$ rdesktop x.x.x.x
```

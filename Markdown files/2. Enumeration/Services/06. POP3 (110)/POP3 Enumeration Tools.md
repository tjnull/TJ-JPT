# POP3 Enumeration Tools

### Nmap Enumeration
```
$ ls -lh /usr/share/nmap/scripts/ | grep pop
-rw-r--r-- 1 root root  3953 Oct 12 09:29 pop3-brute.nse
-rw-r--r-- 1 root root  1397 Oct 12 09:29 pop3-capabilities.nse
-rw-r--r-- 1 root root  4941 Oct 12 09:29 pop3-ntlm-info.nse
$ nmap x.x.x.x -p 110 -sV --script=exampleScript1.nse,exampleScript2.nse
```

### Manual Connection
```
$ telnet x.x.x.x
```
or 
```
$ nc -nv x.x.x.x
```
# FTP Enumeration Tools

### Nmap Enumeration
```
$ ls -lh /usr/share/nmap/scripts/ | grep ftp
-rw-r--r-- 1 root root 4.5K Oct 12 09:29 ftp-anon.nse
-rw-r--r-- 1 root root 3.2K Oct 12 09:29 ftp-bounce.nse
-rw-r--r-- 1 root root 3.1K Oct 12 09:29 ftp-brute.nse
-rw-r--r-- 1 root root 3.2K Oct 12 09:29 ftp-libopie.nse
-rw-r--r-- 1 root root 3.3K Oct 12 09:29 ftp-proftpd-backdoor.nse
-rw-r--r-- 1 root root 3.7K Oct 12 09:29 ftp-syst.nse
-rw-r--r-- 1 root root 5.9K Oct 12 09:29 ftp-vsftpd-backdoor.nse
-rw-r--r-- 1 root root 5.8K Oct 12 09:29 ftp-vuln-cve2010-4221.nse
-rw-r--r-- 1 root root 5.7K Oct 12 09:29 tftp-enum.nse
$ nmap x.x.x.x -p 21 -sV --script=exampleScript1.nse,exampleScript2.nse
```
### Manual Connection
```
$ ftp x.x.x.x
```
```
$ nc x.x.x. 21
```
### Connect via Browser
```
ftp://x.x.x.x
```
# SMTP Enumeration Tools

## Nmap Enumeration

```sh
$ ls -lh /usr/share/nmap/scripts/ | grep smtp
-rw-r--r-- 1 root root  4309 Oct 12 09:29 smtp-brute.nse
-rw-r--r-- 1 root root  4769 Oct 12 09:29 smtp-commands.nse
-rw-r--r-- 1 root root 12006 Oct 12 09:29 smtp-enum-users.nse
-rw-r--r-- 1 root root  5873 Oct 12 09:29 smtp-ntlm-info.nse
-rw-r--r-- 1 root root 10148 Oct 12 09:29 smtp-open-relay.nse
-rw-r--r-- 1 root root   716 Oct 12 09:29 smtp-strangeport.nse
-rw-r--r-- 1 root root 14781 Oct 12 09:29 smtp-vuln-cve2010-4344.nse
-rw-r--r-- 1 root root  7719 Oct 12 09:29 smtp-vuln-cve2011-1720.nse
-rw-r--r-- 1 root root  7603 Oct 12 09:29 smtp-vuln-cve2011-1764.nse
$ nmap x.x.x.x -p 25 -sV --script=exampleScript1.nse,exampleScript2.nse
```

## Manual Connection

```sh
$ nc -nv x.x.x.x 25
```

## Mass email

If you've collected emails from the target domain, you can use something like the following to send out super simple phishing emails. (Saw this on a HTB machine, keep expectations of success low in the real world)

```sh
$ while read mail; do swaks –to $mail –from IT@targetdomain.com –header "Subject: Credentials / Errors" –body "goto http://attackerIP/" –server x.x.x.x; done < mails.txt
```
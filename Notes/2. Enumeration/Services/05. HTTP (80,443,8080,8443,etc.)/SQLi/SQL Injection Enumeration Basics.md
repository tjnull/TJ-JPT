# SQL Injection Enumeration Basics

First, before you forget GET SCREENSHOTS! 
 
Now you basically follow the same standard methodology we always do when attacking a new system. 
- Gather Information
- Enumerate
- Research
- Exploit
- Pivot
- Repeat


The big questions we need to answer to determine the true impact are:
* What is the data exposure?
  - User accounts?
  - Passwords?
  - PII?
  - HPII?
  - Classified data?
  - Configurations?
* What is our access level?
  - Are we a database administrator?
  - Are we unprivileged?
  - Can we modify data through the query? - Sometimes we cannot do nested queries, and are effectively read-only


Some of the information you should obtain first is:
- Database type (Oracle/SQL Server/MySQL/DB2/etc)
- Database version (full banner)
- Current database user and groups
- Host operating system

Then you want to enumerate the database, pull down a list of:
- Databases
- Tables
- Table Schemas
- Stored procedures
- Database Users
- Database User Permissions
- Active connections
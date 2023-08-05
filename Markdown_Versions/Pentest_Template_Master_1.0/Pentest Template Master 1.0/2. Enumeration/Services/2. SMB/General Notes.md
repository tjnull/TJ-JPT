
## Enumerate SMB:

Enum4linux:

- Enum4linux -a 172.21.0.0

SMBmap:

- smbmap -H 172.21.0.0 -d [domain] -u [user] -p [password]
- smbmap -H 172.21.0.0 -d [domain] -u "" -p ""

Nmap: 

- nmap --script smb-* -p 139,445, 172.21.0.0
- nmap --script smb-enum-* -p 139,445, 172.21.0.0

/usr/share/nmap/scripts/smb-brute.nse
/usr/share/nmap/scripts/smb-enum-domains.nse
/usr/share/nmap/scripts/smb-enum-groups.nse
/usr/share/nmap/scripts/smb-enum-processes.nse
/usr/share/nmap/scripts/smb-enum-services.nse
/usr/share/nmap/scripts/smb-enum-sessions.nse
/usr/share/nmap/scripts/smb-enum-shares.nse
/usr/share/nmap/scripts/smb-enum-users.nse
/usr/share/nmap/scripts/smb-flood.nse
/usr/share/nmap/scripts/smb-ls.nse
/usr/share/nmap/scripts/smb-mbenum.nse
/usr/share/nmap/scripts/smb-os-discovery.nse
/usr/share/nmap/scripts/smb-print-text.nse
/usr/share/nmap/scripts/smb-protocols.nse
/usr/share/nmap/scripts/smb-psexec.nse
/usr/share/nmap/scripts/smb-security-mode.nse
/usr/share/nmap/scripts/smb-server-stats.nse
/usr/share/nmap/scripts/smb-system-info.nse


SMBClient: 

- smbclient -L 172.21.0.0
- smbclient //172.21.0.0/tmp

Impacket SmbClient: 

- /usr/share/doc/python3-impacket/examples/smbclient.py username@172.21.0.0

RPCclient: 

- rpcclient -U "" -N 172.21.0.0 enumdomusers

Impacket: 

- python3 samdump.py SMB 172.21.0.0

CrackMapExec: 

- crackmapexec smb -L 
- crackmapexec 172.21.0.0 -u Administrator -H [hash] --local-auth
- crackmapexec 172.21.0.0 -u Administrator -H [hash] --share
- crackmapexec smb 172.21.0.0/24 -u user -p 'Password' --local-auth -M mimikatz

 


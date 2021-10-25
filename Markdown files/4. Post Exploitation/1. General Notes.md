## Spawn a tty: 

1. rlwrap nc localhost 80

2. rlwrap -r -f . nc <IP ADDRESS> <PORT>

- socat file:`tty`,raw,echo=0 tcp-listen:12345
- /bin/sh -i
- /bin/bash -i
- python -c 'import pty; pty.spawn("/bin/sh")'
- perl -e 'exec "/bin/sh";'
- perl: exec "/bin/sh";
- ruby: exec "/bin/sh"
- lua: os.execute('/bin/sh')

## Priviledge Escalation Scripts:

Windows:
- Windows Exploit Suggester (Next-Generation): https://github.com/bitsadmin/wesng
- Sherlock: https://github.com/rasta-mouse/Sherlock
- Powersploit: https://github.com/PowerShellMafia/PowerSploit
- WinPeas: https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS
- PrivescCheck: https://github.com/itm4n/PrivescCheck 

Linux:
- Linux Exploit Suggester 2: https://github.com/jondonas/linux-exploit-suggester-2
- LinEnum: https://github.com/rebootuser/LinEnum
- UnixPriv Checker: https://github.com/pentestmonkey/unix-privesc-check
- LinPeas: https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS

## Other Resources: 

PowerSharpPack:
- https://github.com/S3cur3Th1sSh1t/PowerSharpPack 

Windows: 
- LOLBAS: https://lolbas-project.github.io/#
- Windows Privilege Escalation Fundmentals: https://www.fuzzysecurity.com/tutorials/16.html
- SharpSuite: https://github.com/FuzzySecurity/Sharp-Suite
- Watson: https://github.com/rasta-mouse/Watson
- WinPwn: https://github.com/S3cur3Th1sSh1t/WinPwn

Linux: 
- GTFOBins: https://gtfobins.github.io/
- g0tmi1k Linux Privilege Escalation: https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/
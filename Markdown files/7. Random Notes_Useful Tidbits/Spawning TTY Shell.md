# Spawning TTY Shell

Python:
```
python -c 'import pty; pty.spawn("/bin/bash")'
python3 -c 'import pty; pty.spawn("/bin/bash")'
``````

Bash:
```
echo os.system('/bin/bash')  
/bin/sh -i  
```

Perl:

```
perl -e 'exec "/bin/sh";'  
perl: exec "/bin/sh";  
```

From within vi:

```
:!bash
```

or

```
:set shell=/bin/bash:shell  
```

From within nmap

```
!sh
```
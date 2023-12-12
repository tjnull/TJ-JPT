# Adding Users

Windows:
1. created user :

```
net user hacker hacker93 /add
```

2. Added user to admin:

```
net localgroup administrators hacker /add
```


Linux: (view groups: `cat /etc/group`)

```
adduser hacker # (alt, use full path: /usr/sbin/adduser hacker)
passwd hacker
useradd -G {group-name} hacker
```

Exaample: add user to sudo:

```
/usr/sbin/usermod -aG sudo hacker
```
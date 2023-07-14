# Password List - Generate quick list 

Say you have a simple list of passwords:

```
January
February
March
April
May
June
July
August
September
October
November
December
Password
P@ssw0rd
htb
Secret
Autumn
Fall
Spring
Winter
Summer
```

You can use the following to add the years to all of them:

```
$ for i in $(cat pwlist.txt); do echo $i; echo ${i}2019; echo ${i}2020; echo ${i}\!; done > updated_pwlist.txt
```

You can then use hashcat to mutate the password list:

```
$ hashcat --force --stdout updated_pwlist.txt -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule | sort -u | awk 'length($0) > 8'
```

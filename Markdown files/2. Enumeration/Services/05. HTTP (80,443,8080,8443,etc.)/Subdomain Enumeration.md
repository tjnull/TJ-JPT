# Subdomain Enumeration

### Brute Force with ffuf:
```
$ ffuf -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u http://sneakycorp.htb/ -H "Host: FUZZ.sneakycorp.htb" -fs 185
```

FFuF:
- `ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://172.21.0.0`
- `ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -b "COOKIE VALUE; security=low" -u http://172.21.0.0`
- `ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://172.21.0.0 -fc 403, 302, 200`
- `ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -H "Host: 172.21.0.0" -u http://172.21.0.0`
- `ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://172.21.0.0 -timeout 5`

### sublist3r
```
sublist3r -d google.com
```

### subfinder

```
subfinder -d example.com
```

Add keys to:

`/home/<user>/.config/subfinder/config.yaml`
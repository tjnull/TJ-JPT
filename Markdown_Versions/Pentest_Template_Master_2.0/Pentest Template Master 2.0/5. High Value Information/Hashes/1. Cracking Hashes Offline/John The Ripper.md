DICTIONARY ATTACK
- john --format=#type --wordlist=dict.txt hash.txt

BRUTEFORCE ATTACK
- john --format=#type hash. txt

MASK ATTACK
- john --format=#type --mask=?l?l?l?l?l?l hash.txt -min-len=6

INCREMENTAL ATTACK
- john --incremental hash.txt

DICTIONARY + RULES ATTACK
- john --format=#type --wordlist=dict.t


Other Notes:

BENCHMARK TEST
- john --test

SESSION NAME
- john hash.txt --session=example_name

SESSION RESTORE
- john --restore=example_name

SHOW CRACKED RESULTS
- john hash.txt --pot=<john potfile> --show

WORDLIST GENERATION
- john --wordlist=dict.txt --stdout --external:[filter name] > out.txt

CRACKING SSH KEYS:

- /usr/share/john/ssh2john.py id_rsa > hash.john
- john --wordlist=/usr/share/wordlists/rockyou.txt hash.john

CRACKING KRB5TGS KEYS

- john --format=krb5tgs --wordlist=<passwords_file krb-key.txt

Cracking ASREP Keys

- john --format=krb5asrep --wordlist=<passwords_file asrep-key.txt




## BENCHMARK TEST (HASH TYPE)

- hashcat -b -m #type

## SHOW EXAMPLE HASH

- hashcat -m #type --example-hashes

## DICTIONARY ATTACK

- hashcat -a 0 -m #type hash.txt dict.txt

DICTIONARY + RULES ATTACK

- hashcat -a 0 -m #type hash.txt dict.txt -r rule.txt

COMBINATION ATTACK

- hashcat -a 1 -m #type hash.txt dict1.txt dict2.txt

## MASK ATTACK

- hashcat -a 3 -m #type hash.txt ?a?a?a?a?a?a

HYBRID DICTIONARY + MASK

- hashcat -a 6 -m #type hash.txt dict.txt ?a?a?a?a

HYBRID MASK + DICTIONARY

- hashcat -a 7 -m #type hash.txt ?a?a?a?a dict.txt


## INCREMENT

DEFAULT INCREMENT

- hashcat -a 3 -m #type hash.txt ?a?a?a?a?a --increment

INCREMENT MINIMUM LENGTH

- hashcat -a 3 -m #type hash.txt ?a?a?a?a?a --increment-min=4

INCREMENT MAX LENGTH

- hashcat -a 3 -m #type hash.txt ?a?a?a?a?a?a --increment-max=5

SESSION RESTORE

- hashcat -a 0 -m #type --restore --session <uniq_name> hash.txt dict.txt


## Cracking krb5ts Keys

- hashcat -m 13100 --force <TGSs_file> <passwords_file>

## Cracking Asrep keys

- hashcat -a 0 -m 18200 <asrep_file> <password_file> 


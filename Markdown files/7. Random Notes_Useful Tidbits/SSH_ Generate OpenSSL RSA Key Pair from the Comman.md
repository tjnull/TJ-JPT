# SSH: Generate OpenSSL RSA Key Pair from the Command Line

https://rietta.com/blog/openssl-generating-rsa-key-from-command/
https://en.wikibooks.org/wiki/Cryptography/Generate_a_keypair_using_OpenSSL

You can generate a public and private RSA key pair like this:

`openssl genrsa -aes256 -out private.pem 2048`

Public key:

`openssl rsa -in private.pem -outform PEM -pubout -out public.pem`

Use key for ssh:
1. Copy private.pem to a text file: i.e. key.txt (note you don't HAVE to create a new file. You can do steps 2 and 3 with the private.pem).
2. Set permission to not be too open: 
    - chmod 400 key.txt
3. Ssh with that key:
    - ssh -i key.txt demo@192.168.1.150

Using ssh-keygen
(github example)

`$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

From <https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent> 

Then add the key to ssh:
`$ ssh-add ~/.ssh/id_rsa`

### SSH: using keyfile

`$ ssh -i ssh_key.txt user@IP`
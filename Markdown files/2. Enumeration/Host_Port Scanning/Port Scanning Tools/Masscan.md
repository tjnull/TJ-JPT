# Masscan

Masscan was created to scan the Internet and the author claims it can in under 5 minutes. It's usage is similar to nmap. 
```
masscan x.x.x.x/16 -p0-65535 -oX masscan_x.x.x.x-all_ports.xml
```
You can think of masscan as having the following nmap equivalent settings permanently enabled:
	• `-sS`: this does SYN scan only
	• `-Pn`: doesn't ping hosts first, which is fundamental to the async operation
	• `-n`: no DNS resolution happens
	• `--randomize-hosts`: scan completely randomized
	• `--send-eth`: sends using raw libpcap

Fuse does have a masscan XML parser, so outputting results in XML format is vital for parsing and utilizing Fuse. 

Masscan's default rate is 100 packets/second. To speed it up, add and specify the --max-rate flag:
```
--max-rate 1000
```
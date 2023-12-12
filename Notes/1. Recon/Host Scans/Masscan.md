# Masscan

Masscan was created to scan the Internet and the author claims it can in under 5 minutes. It's usage is similar to nmap. 

```sh
masscan x.x.x.x/16 -p0-65535 -oX masscan_x.x.x.x-all_ports.xml
```

You can think of masscan as having the following nmap equivalent settings permanently enabled:
- `-sS`: this does SYN scan only
- `-Pn`: doesn't ping hosts first, which is fundamental to the async operation
- `-n`: no DNS resolution happens
- `--randomize-hosts`: scan completely randomized
- `--send-eth`: sends using raw libpcap

Fuse does have a masscan XML parser, so outputting results in XML format is vital for parsing and utilizing Fuse. 

Masscan's default rate is 100 packets/second. To speed it up, add and specify the `--max-rate` flag:

`--max-rate 1000`

## Scanning targets
- `masscan 172.21.10.0`
- `masscan 172.21.10.0/24 172.21.0.0/16`
- `masscan 172.21.10.0/24 --excludeFile <File>`
- `masscan 172.21.10.0/24 --exclude 172.21.10.254`

## Scanning for services: 
- `masscan  172.21.10.1 -p 80`
- `masscan  172.21.10.1 -p 0-65535`
- `masscan  172.21.10.1 -p 80,443`
- 
```masscan 172.21.10.0/24 -p 0-65535 --rate 1000000 --open-only --http-user-agent \
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36\
 -oL "output.txt"
```

## UDP Scan
- `masscan 172.21.10.1 -pU 53`

## Report only open ports
- `masscan 10.0.0.1 --open-only`

## Offline Mode (Reviews how fast the program runs without the transmit overhead)
- `masscan 0.0.0.0/24 --offline`

## Obtaining Service banners:
- `masscan 172.21.10.1 --banners`

## Set masscan to use a source ip
- `masscan 10.0.0.1 --source-ip 192.168.1.200`

## Change the default user agent
- `masscan 10.0.0.1 --http-user-agent <user-agent>`

## Save sent packet in PCAP
- `masscan 10.0.0.1 --pcap <filename>`

## References:
- https://github.com/robertdavidgraham/masscan
- https://danielmiessler.com/study/masscan/
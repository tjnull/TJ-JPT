# OS Enumeration - Ping

Pinging a host, if the TTL is 127 (or between 64 and 128) it's likely a windows box.
If the TTL is 64 (or below 64) it's likely a Linux box. (but could be openbsd or other *nix like OSs)
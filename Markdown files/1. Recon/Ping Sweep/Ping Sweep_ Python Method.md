# Ping Sweep: Python Method

The following python script can be used to perform a ping scan. 
```
#!/usr/bin/env python3
import ipaddress
from subprocess import Popen, DEVNULL

for ping in range(1, 254):
        address = "x.x.x.%d" % ping
        response = Popen(["ping", "-c1", address], stdout=DEVNULL)
        output = response.communicate()[0]
        val1 = response.returncode
        if val1 == 0:
                print(address)
```
This script is specifically used for a /24 network. Modification required for other network types. 
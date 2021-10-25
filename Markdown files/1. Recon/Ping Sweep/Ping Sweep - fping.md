# Ping Sweep - fping

#### Fping Multiple IP Address

The below command will fping multiple **IP** address at once and it will display status as alive or unreachable.

```
# fping 50.116.66.139 173.194.35.35 98.139.183.24

50.116.66.139 is alive
173.194.35.35 is unreachable
98.139.183.24 is unreachable
```

#### Fping Range of IP Address

The following command will fping a specified range of IP addressees. With below output we are sending echo request to range of IP address and getting reply as we wanted. Also cumulative result shown after exit.

```
# fping -s -g 192.168.0.1 192.168.0.9 

192.168.0.1 is alive
192.168.0.2 is alive
ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.3
ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.3
ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.3
ICMP Host Unreachable from 192.168.0.2 for ICMP Echo sent to 192.168.0.4
192.168.0.3 is unreachable
192.168.0.4 is unreachable

8      9 targets
       2 alive
       2 unreachable
       0 unknown addresses

       4 timeouts (waiting for response)
       9 ICMP Echos sent
       2 ICMP Echo Replies received
      2 other ICMP received

 0.10 ms (min round trip time)
 0.21 ms (avg round trip time)
 0.32 ms (max round trip time)
        4.295 sec (elapsed real time)
```

#### Fping Complete Network with Different Options

With above command, it will ping complete network and repeat once (**\-r 1**). Sorry, it’s not possible to show output of the command as it is scrolling up my screen with no time.
```
# fping -g -r 1 192.168.0.0/24
```

#### Reads the List of Targets From a File

We have create a file called **fping.txt** having IP address (**173.194.35.35** and **98.139.183.24**) to fping.
```
# fping < fping.txt

173.194.35.35 is alive
98.139.183.24 is alive
```

Suppress the error messsages with `2>/dev/null`
```
fping -a -g 192.168.2.1/24 2>/dev/null
```
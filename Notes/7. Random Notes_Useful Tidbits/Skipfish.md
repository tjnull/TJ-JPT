# Skipfish

https://tools.kali.org/web-applications/skipfish

## Brute forcing and updating Wordlists

Predefined wordlists can be found in `/usr/share/skipfish/dictionaries`

Be sure to make a copy of the wordlist to be used since skipfish will update the wordlist based on scan results. 

Basic command:

`skipfish -o /root/Desktop/skipfish -S medium.wl -W test.wl http://example.com`

- -o = output directory (needs to be created first)
- -S = existing wordlist to be used
- -W = the new wordlist which can be used to store new learnt words

Looking in the output directory, pivtos.txt will contain all the visited URLs. 

Other flags:

-d = to define the depth of crawling (max 16)
-Z = to stop crawling error pages 
--no-injection-tests = don't perform injection tests while scanning 
-I = include these URLs in the scan
-X = don’t include/reject URLs in the scan 
-D scan additional hosts or scan wildcard domains 

## Scan without brute forcing:

`skipfish -LY -o /root/Desktop/skipfish http://target.com`
-L = No keyword learning
-Y= no extension brute forcing 


## Proxy SkipFish through Burp:

Configuring SkipFish:

`skipfish –F www.example.com=10.10.41.103 –o test_output http://www.example.com`

The “-F” option tells SkipFish to send all requests destined for www.example.com to the IP address 192.168.200.50 and not the example.com’s real IP address. 
This is the first step which will ensure that SkipFish will send its requests to the machine on which BurpSuite is running. Now we still need to configure the Burp Suite Proxy so that it will accept and process the requests send by SkipFish.

Configuring Burp Suite

Since the Burp Suite Proxy will be mimicking a real web server in our setup, it needs to listen on the same port as the web application you are testing.
- If you are testing http://www.example.com, then the proxy needs to listen on port 80.
- If you are testing https://www.example.com, then the proxy needs to listen on port 443.
- If you are testing http://www.example.com:8080, then the proxy needs to listen on port 8080.

![SFBurp](SFBurp.png)

From <https://www.vanstechelman.eu/security/using_skipfish_through_burpsuite> 

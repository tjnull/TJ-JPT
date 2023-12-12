# Python burp proxy

```
    #burp proxy
    p = {'http': 'http://127.0.0.1:8080', 'https': 'http://127.0.0.1:8080'}
    r = requests.Session()
    r.proxies.update(p)

	final_request = r.post(stuff)
```


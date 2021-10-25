# Kerberos: Get KDC name and DNS name

```
nslookup -type=srv _kerberos._tcp.REALM
```

Get domain name:
```
Systeminfo | findstr /B /C:"Domain"
```

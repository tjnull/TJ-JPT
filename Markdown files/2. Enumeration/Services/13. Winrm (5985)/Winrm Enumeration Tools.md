# Winrm Enumeration Tools

## evil-winrm

If you were able to crack or recover a password and the winrm service is open, you can use evil-winrm to get a shell (if the user you cracked the password for has permissions):

```
$ evil-winrm -i <target_IP> -u <username> -p <password>
```
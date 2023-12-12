# Enable RDP - Windows

```
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f (might not need this command)

netsh firewall set service remoteadmin enable 
netsh firewall set service remotedesktop enable
```
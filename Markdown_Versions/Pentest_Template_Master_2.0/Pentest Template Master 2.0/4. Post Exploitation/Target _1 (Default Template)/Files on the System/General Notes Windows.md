# Sensitive Files to look for: 

## Windows: 

```
%windir%\repair\sam
%windir%\System32\config\RegBack\SAM
%windir%\repair\system
%windir%\repair\software
%windir%\repair\security
%windir%\debug\NetSetup.log (AD domain name, DC name, internal IP, DA account)
%windir%\iis6.log (5,6 or 7)
%windir%\system32\logfiles\httperr\httperr1.log
C:\sysprep.inf
C:\sysprep\sysprep.inf
C:\sysprep\sysprep.xml
%windir%\Panther\Unattended.xml
C:\inetpub\wwwroot\Web.config
%windir%\system32\config\AppEvent.Evt (Application log)
%windir%\system32\config\SecEvent.Evt (Security log)
%windir%\system32\config\default.sav
%windir%\system32\config\security.sav
%windir%\system32\config\software.sav
%windir%\system32\config\system.sav
%windir%\system32\inetsrv\config\applicationHost.config
%windir%\system32\inetsrv\config\schema\ASPNET_schema.xml
%windir%\System32\drivers\etc\hosts (dns entries)
%windir%\System32\drivers\etc\networks (network settings)
%windir%\system32\config\SAM (only really useful if you have access to the files while the machine is off)
%windir%\unattend.xml
%windir%\Windows\Panther\Unattend.xml
%windir%\Windows\Panther\Unattend\Unattend.xml
%windir%\Windows\system32\sysprep.inf
%windir%\Windows\system32\sysprep\sysprep.xml
C:\ProgramData\Configs\*
C:\Program Files\Windows PowerShell\*
dir c:*vnc.ini /s /b
dir c:*ultravnc.ini /s /b
```

## Search for contents contained in a file:

```
cd C:\ & findstr /SI /M "password" *.xml *.ini *.txt
findstr /si password *.xml *.ini *.txt *.config
findstr /spin "password" *.*
```

## Search for a file with a certain filename:

```
dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config*
where /R C:\ user.txt
where /R C:\ *.ini
```





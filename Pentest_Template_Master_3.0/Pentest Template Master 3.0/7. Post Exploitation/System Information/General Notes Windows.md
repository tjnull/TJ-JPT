# Command Line: 

- systeminfo

# PowerShell

- Get-ComputerInfo
- Get-ComputerInfo -Property "*version"
- Get-ComputerInfo -Property "*version", "os*" | select WindowsCurrentVersion, WindowsVersion, OsName, OsBuildNumber, OsHotFixes, OsArchitecture | fl
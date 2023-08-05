# PowerShell

```
Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize
```

## Obtaining a list of programs from a remote system:

- ```Invoke-command -computer remote_pc_name {Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table -AutoSize }```

## Here is a script that will pull a list of software that is installed on the users system:

```
$listsoftware= Get-ChildItem HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall

$names = $listsoftware |foreach-object {Get-ItemProperty $_.PsPath}

foreach ($name in $names)
{
    Write-Host $name.Displayname
}
```

## WMI:

- ```Get-WmiObject -Class Win32_Product | Select-Object -Property Name > C:\InstalledSoftwareList.txt ```

## Reviewing Installed Windows Features

- ```Get-WindowsFeature | Where-Object {$_.InstallState -eq 'Installed'}```

# Wmic 

## Note: Microsoft has planned to deprecrate this program in new versions of Windows. The commands used can be slow to run but it will return the results it needed:

- wmic /output:C:\InstalledSoftwareList.txt product get name,version

## Saving it to a text file:

- wmic product get name,version /format:csv > C:\InstalledSoftware.csv
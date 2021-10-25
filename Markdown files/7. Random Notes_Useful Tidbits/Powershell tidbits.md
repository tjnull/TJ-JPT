# Powershell tidbits

### List Drives

`get-psdrive -psprovider filesystem`

- From <https://www.thewindowsclub.com/list-drives-using-command-prompt-powershell-windows> 

### Bypass Execution Policy

https://blog.netspi.com/15-ways-to-bypass-the-powershell-execution-policy/

Scenario: Workstation COE with local admin rights

1. **Set the ExcutionPolicy for the CurrentUser Scope via the Registry**
- In this example I’ve shown how to change the execution policy for the current user’s environment persistently by modifying a registry key directly.
`HKEY_CURRENT_USER\Software\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell`

![BEP](../_resources/BEP.png)

---

#powershell 
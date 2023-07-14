# Ping Sweep: PowerShell Method

Using PowerShell as the method for ping sweeping is useful if you connect to an internal Windows box and are unable to transfer nmap or perform another form of ping sweep. 

```powershell
1..20 | % {"x.x.x.$($_): $(Test-Connection -count 1 -comp x.x.x.$($_) -quiet)"} 
```

Again, this example is for a /24 network. Modification required for other network types.  
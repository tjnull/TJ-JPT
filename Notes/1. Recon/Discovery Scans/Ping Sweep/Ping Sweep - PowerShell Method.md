# Ping Sweep: PowerShell Method

Using PowerShell as the method for ping sweeping is useful if you connect to an internal Windows box and are unable to transfer nmap or perform another form of ping sweep.

Note: This command can also run on powershell for Linux

```powershell
1..20 | % {"x.x.x.$($_): $(Test-Connection -count 1 -comp x.x.x.$($_) -quiet)"} 
```

Again, this example is for a /24 network. Modification required for other network types. 

Get-PingSweep Subnet 172.21.10:

```powershell
# Reference: https://gist.github.com/joegasper/93ff8ae44fa8712747d85aa92c2b4c78
function ResolveIp($IpAddress) {
    try {
        (Resolve-DnsName $IpAddress -QuickTimeout -ErrorAction SilentlyContinue).NameHost
    } catch {
        $null
    }
}

function Invoke-PingSweep {
    [CmdletBinding()]
    Param(
        [Parameter(Mandatory=$true)]
        [string]$SubNet,
        [switch]$ResolveName
    )
    $ips = 1..254 | ForEach-Object {"$($SubNet).$_"}
    $ps = foreach ($ip in $ips) {
        (New-Object Net.NetworkInformation.Ping).SendPingAsync($ip, 250)
        #[Net.NetworkInformation.Ping]::New().SendPingAsync($ip, 250) # or if on PowerShell v5
    }
    [Threading.Tasks.Task]::WaitAll($ps)
    $ps.Result | Where-Object -FilterScript {$_.Status -eq 'Success' -and $_.Address -like "$subnet*"} 
    Select-Object Address,Status,RoundtripTime -Unique |
    ForEach-Object {
        if ($_.Status -eq 'Success') {
            if (!$ResolveName) {
                $_
            } else {
                $_ | Select-Object Address, @{Expression={ResolveIp($_.Address)};Label='Name'}, Status, RoundtripTime
            }
        }
    }
}
```
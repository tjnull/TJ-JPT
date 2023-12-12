# Ping Sweep: Windows Method

If you don't have access to PowerShell, you can use the following from a Windows command line:

```console
for /L %i in (1,1,255) do @ping -n 1 -w 200 x.x.x.%i > nul && echo x.x.x.%i is up.
```
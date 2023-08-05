# Check for Processes

- tasklist
- wmic process list full

# In PowerShell
- Get-Process
- Get-Process -Name 'Notepad'

List path where the process is running:
- (Get-Process -Name 'Calculator').Path

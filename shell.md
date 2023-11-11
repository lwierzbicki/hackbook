## Shells

Enable-PSRemoting
Set-Item wsman:\localhost\client\trustedhosts 192.168.50.80
Invoke-Command -ComputerName 192.168.50.80 -ScriptBlock { ipconfig } -Credential offensive
Enter-PSSession -ComputerName 192.168.50.80 -Credential offensive
evil-winrm
python3 -c 'import pty;pty.spawn("/bin/bash")'

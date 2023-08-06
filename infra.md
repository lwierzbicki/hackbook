# Table of contents

- [Enumeration](#enumeration)
- [Services](#services)

## Enumeration

Get resolve local addresses in DNS server
```bash
dnsrecon -r 192.168.33.0/24 -n 192.168.33.2 -d domain.corp -c $PWD/output.csv
```

Masscan  on specified interface, all ports and max rate 300
```bash 
masscan -e tun0 -iL ips.txt -p0-65535 --max-rate 300 --open-only -oG scan.txt
```

Nmap scan - TCP/All ports/Service detection/Multi-output
```bash
nmap -Pn -sS -sV -p- -sC 192.168.0.10 -oA output
```
Nmap scan - Addresses from the list/TCP/All ports/Service detection/Multi-output
```bash
nmap -Pn -sV -p- -iL ips.txt -oA output
```
Nmap scan - UDP/top 1000 ports/Service detection/Multi-output
```bash
nmap -Pn -sV -sU 192.168.0.10 -oA output
```


## Services

### LDAP

Run nmap for ldap
```
nmap -p 389 --script=ldap-search ip_address
```

Access without user
```bash
ldapsearch -LLL -x -H ldap://ad.server.local -b '' -s base '(objectclass=*)'
```

Get a list of all domain users
```bash 
ldapsearch -LLL -x -H ldap://ad.server.local -D "user@domain.local" -w 'password' -b "DC=domain,DC=local" "(objectClass=user)" sAMAccountName userPrincipalName memberOf
```

Get a list of all domain groups
```bash
ldapsearch -LLL -x -H ldap://ad.server.local -D "user@domain.local" -w 'password' -b "DC=domain,DC=local" "(objectClass=group)" sAMAccountName member memberOf
```

Get a list of all domain joined systems
```bash
ldapsearch -LLL -x -H ldap://ad.server.local -D "user@domain.local" -w 'password' -b "DC=domain,DC=local" "(objectClass=computer)" name dNSHostname operatingSystem operatingSystemVersion lastLogonTimestamp servicePrincipalName
```

Recursively lookup all members of a group
```bash
ldapsearch -LLL -x -H ldap://ad.server.local -D "user@domain.local" -w 'password' -b "DC=domain,DC=local" "(&(objectClass=user)(memberof=CN=DOMAIN ADMINS,CN=Users,DC=domain,DC=local))" | grep sAMAccountName | cut -d" " -f2
```

Show all groups a user is memberOf
```bash
ldapsearch -LLL -x -H ldap://ad.server.local -D "user@domain.local" -w 'password' -b "DC=domain,DC=local" "(sAMAccountName=user)" sAMAccountName userPrincipalName memberOf | grep memberOf | cut -d "=" -f2 | cut -d"," -f1
```

Run bloodhound to get data
```bash
bloodhound-python -dc dc.domain.local -gc dc.domain.local -ns 192.168.0.1 -u user@domain.local -p password -d domain.local --zip --disable-autogc --auth-method ntlm -c all
```


### SMB

Null Session
```bash
rpcclient -U "" [ip_address]
```

Connect SMB without password prompt
```bash
smbclient -N -L \\\\192.168.0.1
```
Connect to SMB share users as bob
```bash
smbclient -U bob \\\\192.168.0.1\\users
```
Connect as domain user to SMB sysvol
```bash
smbclient -U domain.local/user%password \\\\192.168.0.1\\SYSVOL
```
Mount drive to /mnt/winshare
```bash
sudo mount -t cifs -o username=user@domain.local //192.168.0.1/SYSVOL /mnt/winshare
```

nbtscan -r [ip_address / ip range]
enum4linux -v [ip_address]
smbmap
crackmapexec
nmblookup -A '[ip address]'

### SNMP
onesixtyone -c [filename community string]
//windows snmp enumeration
snmpwalk -c [community string] -v1 [ip_address]
//snmp enumerate
snmp-check
snmp-check 192.168.0.10 -c public
snmpcheck ###
snmpenum ###

## WIFI

### NTML
In wireshark recognize NTLMSSP on other ports by running lua script
```lua
local tcp_port_table = DissectorTable.get("tcp.port")
local tcp_ntlmssp_dis = Dissector.get("ntlmssp")
tcp_port_table:add(6901, tcp_ntlmssp_dis)
```

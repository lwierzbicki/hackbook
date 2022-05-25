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
masscan -e ens4 -iL ips.txt -p0-65535 --max-rate 300 -oG scan.txt
```
Nmap scan - TCP/All ports/Service detection/Multi-output
```bash
nmap -Pn -sV -p- 192.168.0.10 -oA output
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

### NTML
In wireshark recognize NTLMSSP on other ports by running lua script
```lua
local tcp_port_table = DissectorTable.get("tcp.port")
local tcp_ntlmssp_dis = Dissector.get("ntlmssp")
tcp_port_table:add(6901, tcp_ntlmssp_dis)
```

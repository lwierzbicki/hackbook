# Table of contents

- [Enumeration](#enumeration)


## Enumeration

Get resolve local addresses in DNS server
```bash
dnsrecon -r 192.168.33.0/24 -n 192.168.33.2 -d domain.corp -c $PWD/output.csv
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

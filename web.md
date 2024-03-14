# Table of contents

- [Enumeration](#enumeration)

## Enumeration

gobuster dir -u http://'[ip address]'/ -w /usr/share/seclists/Discovery/Web-Content/common.txt
gobuster dir -u http://'[ip address]'/ --proxy http://127.0.0.1:8080 -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 1

whatweb http://'[ip address]'/

wget -r -np http://192.168.50.129/.git/

## SQLi

sqlmap --proxy="http://127.0.0.1:8080" -r req1 --level=3 --risk=3 --force-ssl --tamper space2comment,randomcomments
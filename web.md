# Table of contents

- [Enumeration](#enumeration)

## Enumeration

gobuster dir -u http://'[ip address]'/ -w /usr/share/seclists/Discovery/Web_Content/common.txt

gobuster dir -x php -u http://10.129.113.219/ -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt
sudo docker run -v /usr/share/wordlists/dirbuster/:/wordlist/ -it ghcr.io/xmendez/wfuzz wfuzz -w directory-list-1.0.txt  --hc 302,400,401,404,500 https://104.242.9.204/FUZZ

wget -r -np http://192.168.50.129/.git/

wfuzz -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt http://10.129.199.248/FUZZ
gobuster dir -x php -u http://10.129.199.248/ -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt

## SQLi

 sqlmap --proxy="http://127.0.0.1:8080" -r req1 --level=3 --risk=3 --force-ssl
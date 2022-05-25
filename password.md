## johnny
Random rule
```bash
john -format='dynamic=md5($c1.$p.$c2),c1=T,c2=1g!$_' [file_with_hashes] --wordlist=[wordlist_location]
```


## Hashcat
Unix hashes
```bash
hashcat -m '[module]' -a 0 --remove kioptrix13_hash /usr/share/wordlists/rockyou.txt -o kioptrix13_password -a 0 dictionary
```

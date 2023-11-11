## Transfer

### FTP

apt-get install pure-ftpd
//tools_ftp_setup.sh

curl -u [user]:[pass] -T collect.txt ftp://10.11.0.98/

echo user [user] [pass] >> ftp.txt
echo put 238.txt >> ftp.txt
echo close >> ftp.txt
echo bye >> ftp.txt
'ftp -inv 10.11.0.90 << ftp.txt'

### HTTP

python -m SimpleHTTPServer '[port]'
python3 -m http.server '[port]'

### TFTP

mkdir /tftp
atftpd --daemon --port 69 /tftp/
client: tftp -i '[ip address]' get '[file]'
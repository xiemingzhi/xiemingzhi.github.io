---
title: "Setting up transmission on entware"
description: "transmission web entware"
category: 
tags: [transmission, entware]
---
{% include JB/setup %}

Make sure you have setup entware properly, follow my blog [Adventures with rt-ac55uhp and entware](). Requirements able to reboot and start entware services automatically.

Update entware packages:
```
opkg update
```

Install transmission daemon and web:
```
opkg install transmission-daemon-openssl
opkg install transmission-web
```

transmission settings file is located at:  
/opt/etc/transmission/settings.json

Make sure you have inserted/mounted a drive with free space for your torrents:
```
df -h
```
Using the asus router web gui create the share folder and create the users and give them permission to access the share folder.

Edit the settings.json file to the location of the share/download folder:
```
"download-dir": "/opt/downloads/torrent",
"incomplete-dir": "/opt/downloads/torrent/incomplete",
"bind-address-ipv4": "0.0.0.0",
"bind-address-ipv6": "::",
"rpc-port": 9091,
"rpc-authentication-required": true,
"rpc-username": "admin",
"rpc-password": "yourpassword",
```

To start the service:
```
/opt/etc/init.d/S88transmission start
```

Access the transmission web gui:  
http://192.168.1.1:9091/transmission/web/

P.S I had trouble downloading content found out had to open some ports on entware:
```
iptables -I INPUT -p tcp --destination-port 51413 -j ACCEPT
iptables -I INPUT -p udp --destination-port 51413 -j ACCEPT

```

No need to restart the service. You can restart the whole router to make sure the service starts on reboot.


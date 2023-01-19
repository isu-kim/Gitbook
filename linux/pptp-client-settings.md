---
description: PPTP Client Settings for CoolIP
---

# PPTP Client Settings

### Install PPTP

```
sudo apt-get install pptp-linux network-manager-pptp
```

### Setup files

* `/etc/ppp/chap-secrets`

```
COOLIP@username * password *
```

* `/etc/ppp/peers/coolip`

```
pty "pptp coolip-s1.coolip.co.kr --nolaunchpppd"
name "COOLIP@username"
remotename PPTP
file /etc/ppp/options.pptp
ipparam COOLIP
```

* `/etc/ppp/ip-up.d/route-traffic`

```
#!/bin/sh
NET="0.0.0.0/0"
IFACE="ppp0"
route add -net ${NET} dev ${IFACE}
```

### Running

* **Execute PPTP using**

```
ppp call coolip
```

Also check if it is working properly as device `ppp0` using `ifconfig ppp0.`

* **Stop PPTP using**

```
poff coolip
```


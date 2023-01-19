---
description: Make OpenVPN Server with Simple Script
---

# OpenVPN Server

### Download Shell

```
wget https://git.io/vpn -O openvpn-install.sh
```

### Change Permission and Execute

```
sudo chmod +x openvpn-install.sh
sudo bash openvpn-install.sh
```

### Afterwards

* This will generate your `.ovpn` client in `/root` directory.&#x20;
* You must open `1194/UDP` when using OpenVPN (default port and protocol)
* Use Tunnelblick for Mac, OpenVPN Client for Windows

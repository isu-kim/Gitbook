---
description: Recover network of a machine where netplan and iproute2 package disappeared
---

# Recover network (no netplan, iproute2)

### 1. Check networkctl

```bash
networkctl status ens18 # change ens18
```

### 2. Update systemd

```bash
sudo mkdir -p /etc/systemd/network
sudo nano /etc/systemd/network/20-wired.network
```

Modify the network config

```editorconfig
[Match]
Name=ens18

[Network]
Address=10.130.0.162/24
Gateway=10.130.0.1
DNS=8.8.8.8
DNS=8.8.4.4
#DHCP=yes # for dhcp
```

### 3. Restart networkd

```bash
sudo systemctl restart systemd-networkd
networkctl status ens18
```

If network works, continue 4.

### 4. (Optional) recover netplan and iproute2

```bash
sudo apt-get install netplan.io -y
sudo apt-get install iproute2 -y
```

Apply netplan

```bash
sudo netplan apply
```

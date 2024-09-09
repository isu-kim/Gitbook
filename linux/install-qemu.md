---
description: Install Qemu and Run Ubuntu
---

# Install Qemu

### 1. Check KVM

```
sudo apt install cpu-checker
kvm-ok
```

Check support for KVM

### 2. Install Qemu

```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
sudo adduser 'username' libvirt
sudo adduser 'kvm' libvirt
```

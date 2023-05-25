---
description: Install Vagrant and VirtualBox
---

# Vagrant and VirtualBox

Provider can be `VirtualBox` or `Qemu`, choose the one that fits into your environment:

> On ESXi VM, VirtualBox seems not to be supported, therefore use qemu.

### 1. Install VirtualBox

Visit [https://www.virtualbox.org/wiki/Linux\_Downloads](https://www.virtualbox.org/wiki/Linux\_Downloads). Find the one that fits into your environment. For me, I am using Ubuntu 22.04, so download `.deb` file.

```
wget https://download.virtualbox.org/virtualbox/7.0.8/virtualbox-7.0_7.0.8-156879~Ubuntu~jammy_amd64.deb
```

Then use `dpkg` (or `yum` for CentOS), for installing the package.

```
sudo dpkg -i virtualbox-7.0_7.0.8-156879~Ubuntu~jammy_amd64.deb
```

When this gets you depdenency errors, fix broken using:

```
sudo apt --fix-broken install
```

Add this current user to `vboxusers` group

```
sudo usermod -aG vboxusers $USER
```

Then verify the installation

```
virtualbox --version
```

### 1. Install Qemu

```
sudo apt-get install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
```

### 2. Install Vagrant

```
sudo apt-get install vagrant
```


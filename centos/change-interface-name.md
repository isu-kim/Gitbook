---
description: Change Network Interface Names
---

# Change Interface Name

By default, CentOS 8 Stream sets the network interfaces like `ens192` or `ens224` or some other names. However, for some cases, we need to manually set interface's names into `eth0` or `eth1` like the old days.

### 1. Modify Grub

Edit `/etc/default/grub` and add `net.ifnames=0` and `biosdevname=0` to the file. Then save the file. Once the file was saved, apply the change using&#x20;

```
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

This will apply the changes into your boot grub system.

### 2. Add Rules to udev

Before you add rules into udev, you need to identify which network mac addresses are assigned. You can perform this by `ifconfig`. Let's say that you have following two interfaces:

* `ens192` having MAC address of `00:0c:29:9a:e1:b0`
* `ens224` having MAC address of `00:0c:29:9a:e1:ba`

Store those MAC addresses somewhere you are able to remember. Then add rules to `/etc/udev/rules.d/`, by adding a new rule named `70-persistent-net.rules`

```
vim /etc/udev/rules.d/70-persistent-net.rules
```

Add following content inside the file:

```
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="MAC_ADDRESS", ATTR{type}=="1", KERNEL=="eth*", name="eth0"
```

Replace `MAC_ADDRESS` with the real MAC address that was retrieved earlier. Also, change `eth0` to some other name that you wish to use. An example shall be like below:

```
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:0c:29:9a:e1:b0", ATTR{type}=="1", KERNEL=="eth*", name="eth0"
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:0c:29:9a:e1:ba", ATTR{type}=="1", KERNEL=="eth*", name="eth1"
```

### 3. Add network-scripts

If you have finished till the step 2 and reboot the system, the interface will have correct name with correct MAC address. However, it will NOT have a IP assigned to the interface. In order to add IPs into the interface, add network scripts by creating a new network-scripts and let the OS do the job.

```
cd /etc/sysconfig/network-scripts
```

Then you will be able to see old network script files that were used before. If you have located those files, rename them and modify the contents accordingly for the new interface. For example, if we had interface named `ens192`, there will be a network script named `ifcfg-ens192`. Just copy the fle into `ifcfg-eth0` and modify some contents inside it:

```
cp ifcfg-ens192 ifcfg-eth0
```

Edit the content like below:

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=eui64
NAME=eth9
UUID=56ef8177-fe28-4cc7-9223-03b01640fec0
DEVICE=eth0
ONBOOT=yes
```

Set DHCP or other optioins accordinlgy, but set `NAME` and `DEVICE` as the ones that we were trying to set into.

> Be aware that latest versions of CentOS like CentOS9, the network-scripts are deprecated. Therefore, look for other ways to automatically assign IP addresses to an interface on the web.

### 4. Reboot

Once step 1 \~ 3 were done, it's time for reboot.  Once, the machine was rebooted, use `ifconfig` and look for the network interfaces:

```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.25.244.82  netmask 255.255.255.0  broadcast 172.25.244.255
        inet6 fe80::20c:29ff:fe9a:e1b0  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:9a:e1:b0  txqueuelen 1000  (Ethernet)
        RX packets 880  bytes 62569 (61.1 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 73  bytes 8767 (8.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.25.244.88  netmask 255.255.255.0  broadcast 172.25.244.255
        inet6 fe80::20c:29ff:fe9a:e1ba  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:9a:e1:ba  txqueuelen 1000  (Ethernet)
        RX packets 820  bytes 53322 (52.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 13  bytes 1446 (1.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Both names of interfaces were changed successfully.&#x20;

'




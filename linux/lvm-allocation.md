---
description: Allocate more disk space to LVM.
---

# LVM Allocation

Sometime you have enough disk space, however the LVM root partition does not have enough disk space:

### Check Disk

```
sudo lshw -class disk
```

```
  *-namespace
       description: NVMe namespace
       physical id: 1
       logical name: /dev/nvme0n1
       size: 931GiB (1TB)
       capabilities: gpt-1.00 partitioned partitioned:gpt
       configuration: guid=fc1d5b22-6f7c-4295-9c83-09f88baf1f3f logicalsectorsize=512 sectorsize=512
```

We have a `1TB` NVMe disk, however the LVM partition shows like below:

```
df -h
```

```
Filesystem                         Size  Used Avail Use% Mounted on
udev                                32G     0   32G   0% /dev
tmpfs                              6.3G  2.0M  6.3G   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv   98G   29G   65G  31% /
tmpfs                               32G   12K   32G   1% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
tmpfs                               32G     0   32G   0% /sys/fs/cgroup
...
```

### Increase LVM

As we can see, the `/dev/mapper/ubuntu--vg-ubuntu--lv` has only 98G allocated. You can increase the size of lvm by 5GB using:

```
sudo lvextend --resizefs -L +5G ubuntu-vg/ubuntu-lv
```

Or you can even allocate 100% of the free disk using:

```
sudo lvextend --resizefs -l +100%FREE ubuntu-vg/ubuntu-lv
```

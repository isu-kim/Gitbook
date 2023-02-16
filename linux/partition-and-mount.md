---
description: Make a partition and mount it to directory
---

# Partition and Mount

### Generate Partition

Let's assume that we have a disk named `/dev/sdb`, then make partition with the disk.&#x20;

```
sudo fdisk /dev/sdb
```

This will bring up the `fdisk` utility. Then we are going to generate partition: Just press `n` and press `enter` (if you do not want any options and are willing to use the whole disk)

```
Command (m for help): n
Partition number (1-128, default 1):
First sector (34-1953525134, default 2048):
Last sector, +sectors or +size{K,M,G,T,P} (2048-1953525134, default 1953525134):

Created a new partition 1 of type 'Linux filesystem' and of size 931.5 GiB.
```

Then use `w` to write information to disk

```
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

### Format Partition

Format partition using `mkfs`:

```
sudo mkfs -t ext4 /dev/sdb1
```

This will generate file system of `/dev/sdb1` as `ext4`. If you would like other file systems, use this accordingly.

### Mount Partition

Once generating partition and formatting a partition was done, it is time for mounting. Let's assume that we would like to mount partition`/dev/sdb1` to `/home2` directory.

```
mkdir /home2
sudo mount -t auto /dev/sdb1 /home2
```

This will mount `/dev/sdb1` to `/home2` directory.

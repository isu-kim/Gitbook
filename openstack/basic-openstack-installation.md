---
description: Basic Installation Procedure for CentOS based OpenStack
---

# \[Basic] OpenStack Installation

Before we use `packstack` or any other commands, we need to have our OpenStack's repos and requirements set properly. Otherwise, we have to roll back our OpenStack environment, which is very painful.

### 0. Tips

1. If your `dnf` is too slow, try adding `add max_parallel_downloads=10` and `fastestmirror=true` to `/etc/dnf/dnf.conf`.
2. If your OpenStack failed, just wipe out the OS and reinstall everything. I would suggest using ESXi or some other virtual devices which supports snapshots before diving into real physical devices and running with USBs with OS image.

### 1. Install Dependencies and Packages

```bash
sudo dnf update -y
sudo dnf install -y centos-release-openstack-yoga  # or zed or anything else
sudo setenforce 0
sudo dnf config-manager --enable powertools  # need this for future use
sudo dnf install -y openstack-packstack
sudo dnf update -y
```

With some CentOS releases, without `powertools`, we are not even able to install `centos-release-openstack-zed` or some other packages. Also, even if you have managed to install yoga in your environment, not having `powertools` will result in failure of installation:

For example, following error will happen due to `dnf install -y openstack-glance` failing due to not having proper repository:

```
Problem: package openstack-glance-1:24.2.0-1.el8.noarch requires python3-glance = 1:24.2.0-1.el8, but none of the providers can be installed
  - cannot install the best candidate for the job
  - nothing provides python3-pyxattr needed by python3-glance-1:24.2.0-1.el8.noarch
```

Also, do not forget to `setenforce 0` if you would not want any more headache.

> When you have followed all commands, check if packages are actually able to be installed. Check this using `sudo dnf install openstack-glance`. If this fails, it means that the repositories were not set properly and PackStack will eventually fail in the future when applying puppet.

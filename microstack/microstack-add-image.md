---
description: Add images to openstack with example of Ubuntu 22.04
---

# \[Microstack] Add Image

### Download Image

```bash
wget http://cloud-images.ubuntu.com/releases/22.04/release/ubuntu-22.04-server-cloudimg-amd64.img
```

We need **cloud images**, not the ones that we normally install with USB stick. For my case, I will be using 22.04 for demo.

### Add image

```bash
microstack.openstack image create "ubuntu2204" --file ubuntu-22.04-server-cloudimg-amd64.img --disk-format qcow2 --container-format bare --public
```

This will add Ubuntu 22.04 image to the openstack.&#x20;

### Troubleshooting

If you happen to encounter

```
[Error 13] Permission denied file ubuntu-22.04-server-cloudimg-amd64.img
```

As an error, move the `.img` file to the directory `/var/snap/microstack/common/images`. Then execute the same command again.


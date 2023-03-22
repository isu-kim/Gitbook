---
description: All trouble shooting for Magnum that I have encountered
---

# \[Magnum] TroubleShooting

### 1. `Failed to create certificates for Cluster`

This occurs due to Magnum unable to generate SSL/TLS certificate. In Magnum, there are three options to generate and store certificates:

* Barbican
* Magnum’s own database
* Local store

Check `/etc/magnum/magnum.conf` and look for `[certificates]` section. Make the `cert_manager_type` as `x509keypair` instead of `barbican` and `local` like below:

```jsx
[certificates]

#
# From magnum.conf
#

# Certificate Manager plugin. Defaults to barbican. (string value)
#cert_manager_type = barbican
#cert_manager_type=local
cert_manager_type = x509keypair
```

Then apply the change by:

```bash
sudo systemctl restart openstack-magnum-api.service
sudo systemctl restart openstack-magnum-conductor.service
```

Check [https://docs.openstack.org/magnum/latest/user/](https://docs.openstack.org/magnum/latest/user/) and look for “**Storing the certificates**[**¶**](https://docs.openstack.org/magnum/latest/user/#storing-the-certificates)**”**

### 2. `unexpected keystone client error occurred: publicURL endpoint for orchestration service in RegionOne region not found`

This occurs due to Magnum not having Heat installed. In order for Magnum to work, we need Heat orchestration service running.

Check [https://www.openstack.org/software/releases/zed/components/magnum](https://www.openstack.org/software/releases/zed/components/magnum) for Magnum’s dependencies.

#### Try

```bash
sudo packstack --install --os-heat-install=y
```

This might or might not work. If the command failed with `Parameter CONFIG_CONTROLLER_HOST failed validation: Given host does not listen on port 22:`, reinstall whole OpenStack using PackStack.

> &#x20;Seriously. I have tried `--os-controller-host` arguments with `sshd` running in the background, but it seems not to recognize the host.

> If you know the exact value for **`PW_PLACEHOLDER`** you can just install it using packstack with creating another `answers.txt` file with the same value, but with skipping compute node and network node.

### 3. `ERROR: The Parameter (octavia_provider) was not defined in template.`

This is due to the compatability issue of OS image and the Magnum cluster’s template. Check [https://stackoverflow.com/questions/56339692/openstack-magnum-error-the-parameter-octavia-ingress-controller-tag-was-not-d](https://stackoverflow.com/questions/56339692/openstack-magnum-error-the-parameter-octavia-ingress-controller-tag-was-not-d) for more information.

There is a compatability matrix ([https://wiki.openstack.org/wiki/Magnum](https://wiki.openstack.org/wiki/Magnum)) which tells you which version goes well with which version. Check the wiki’s matrix.

### 4. I don’t see any images in templates!

According to[https://docs.openstack.org/magnum/latest/user/](https://docs.openstack.org/magnum/latest/user/),

> The name or UUID of the base image in Glance to boot the servers for the cluster. The image must have the attribute ‘os\_distro’ defined as appropriate for the cluster driver. For the currently supported images, the os\_distro names are:

Therefore, the image must have property of `os_distro=fedora-coreos` for Kubernetes. Download OS image from [https://getfedora.org/en/coreos/download?tab=metal\_virtualized\&stream=stable\&arch=x86\_64](https://getfedora.org/en/coreos/download?tab=metal\_virtualized\&stream=stable\&arch=x86\_64). Then build the image using:

```bash
openstack image create "fedora-core" --file fedora-coreos-37.20230218.3.0-qemu.x86_64.qcow2.xz --disk-format qcow2 --container-format bare --public --property os_distro=fedora-coreos
```

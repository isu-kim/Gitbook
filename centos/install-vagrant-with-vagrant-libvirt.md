---
description: Install Vagrant and vagrant-libvirt in CentOS 8
---

# Install Vagrant with vagrant-libvirt

In order to use libvirt related VMs, such includes qemu and kvm, we need to use vagrant-libvirt plugin for Vagrant. To do this, following steps shall be followed

### Install Vagrant

Add repo for Vagrant and install vagrant by:

```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install vagrant
```

### Install Libvirt

Install all dependencies for libvirt as well as libvirt's libraries

```bash
# Install dependencies for libvirt
sudo dnf install --assumeyes libvirt libguestfs-tools \
    gcc libvirt-devel libxml2-devel make pkgconf-pkg-config ruby-devel
sudo dnf install --assumeyes byacc cmake gcc-c++ rpm-build wget zlib-devel
sudo systemctl enable libvirtd 
sudo systemctl start libvirtd
sudo usermod -aG libvirt $USER 
```

In here, if we do not perform `usermod` the libvirt will not create and manage VMs. Therefore, add current user for libvirt.

### Install vagrant-libvirt

In order to use vagrant-libvirt in Vagrant as the provider, use **vagrant-libvirt**.&#x20;

```
vagrant plugin install vagrant-libvirt
```

### Example Vagrantfile

To verify that the vagrant-libvirt was installed properly, use following Vagrantfile to check if it was installed properly:

```
Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"  # Ubuntu 22.04 
  config.vm.box_check_update = false

  config.vm.provider :libvirt do |libvirt|
    # Disable KVM and use pure QEMU
    libvirt.driver = "qemu"

    libvirt.memory = "2048"  # Set the VM memory to 2GB
    libvirt.cpus = 2  # Set the number of CPUs to 2
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    # Additional provisioning steps can be added here
  SHELL
end
```

This will create a VM with ubuntu2204 image and will perform `sudo apt-get update`.

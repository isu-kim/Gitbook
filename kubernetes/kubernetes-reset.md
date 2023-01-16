---
description: Reset Kubernetes alongside with kubeadm
---

# Kubernetes Reset

Using `kubeadm reset` leaves some configurations and settings behind. For example, this will leave `~/.kube/config` which can potentially affect next installation and mess up the workspace. Therefore, cleaning up the workspaces that were used before is necessary for a clean re-installation process.

### Command

```
sudo systemctl stop kubelet
sudo systemctl stop docker

sudo rm -rf $HOME/.kube
sudo rm -rf /etc/cni
sudo rm -rf /var/lib/cni
sudo rm -rf /var/lib/etcd
sudo rm -rf /var/lib/kubelet/*

sudo systemctl start docker
sudo systemctl start kubelet
```

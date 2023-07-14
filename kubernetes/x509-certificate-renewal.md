---
description: Renew x509 Certificate for Kubernetes
---

# x509 Certificate Renewal

When your x509 certificate(s) for Kubernetes was expired, following error will be shown

```bash
$ kubectl get pods -n saas-beta
Unable to connect to the server: x509: certificate has expired or is not yet valid: current time 2023-07-14T01:22:53Z is after 2023-07-10T11:45:22Z
```

### Check Certificates

```bash
$ sudo kubeadm certs check-expiration
[check-expiration] Reading configuration from the cluster...
[check-expiration] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[check-expiration] Error reading configuration from the Cluster. Falling back to default configuration

CERTIFICATE                EXPIRES                  RESIDUAL TIME   CERTIFICATE AUTHORITY   EXTERNALLY MANAGED
admin.conf                 Jul 10, 2023 11:45 UTC   <invalid>       ca                      no
apiserver                  Jul 10, 2023 11:45 UTC   <invalid>       ca                      no
apiserver-etcd-client      Jul 10, 2023 11:45 UTC   <invalid>       etcd-ca                 no
apiserver-kubelet-client   Jul 10, 2023 11:45 UTC   <invalid>       ca                      no
controller-manager.conf    Jul 10, 2023 11:45 UTC   <invalid>       ca                      no
etcd-healthcheck-client    Jul 10, 2023 11:45 UTC   <invalid>       etcd-ca                 no
etcd-peer                  Jul 10, 2023 11:45 UTC   <invalid>       etcd-ca                 no
etcd-server                Jul 10, 2023 11:45 UTC   <invalid>       etcd-ca                 no
front-proxy-client         Jul 10, 2023 11:45 UTC   <invalid>       front-proxy-ca          no
scheduler.conf             Jul 10, 2023 11:45 UTC   <invalid>       ca                      no

CERTIFICATE AUTHORITY   EXPIRES                  RESIDUAL TIME   EXTERNALLY MANAGED
ca                      Jul 07, 2032 11:45 UTC   8y              no
etcd-ca                 Jul 07, 2032 11:45 UTC   8y              no
front-proxy-ca          Jul 07, 2032 11:45 UTC   8y              no
```

The current date is Jul 14, 2023 which is after the expiration of cerificate. Therefore, we need to renew certificates.

### Renew Certificate

According to [https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-certs/](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-certs/) We can use `kubeadm` to renew certificates with ease.

```bash
$ sudo kubeadm certs renew all
[renew] Reading configuration from the cluster...
[renew] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[renew] Error reading configuration from the Cluster. Falling back to default configuration

certificate embedded in the kubeconfig file for the admin to use and for kubeadm itself renewed
certificate for serving the Kubernetes API renewed
certificate the apiserver uses to access etcd renewed
certificate for the API server to connect to kubelet renewed
certificate embedded in the kubeconfig file for the controller manager to use renewed
certificate for liveness probes to healthcheck etcd renewed
certificate for etcd nodes to communicate with each other renewed
certificate for serving etcd renewed
certificate for the front proxy client renewed
certificate embedded in the kubeconfig file for the scheduler manager to use renewed

Done renewing certificates. You must restart the kube-apiserver, kube-controller-manager, kube-scheduler and etcd, so that they can use the new certificates.
```

When it was renewed, check the certificates again.

```bash
$ sudo kubeadm certs check-expiration
[check-expiration] Reading configuration from the cluster...
[check-expiration] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
W0714 01:51:36.018912 1915378 configset.go:177] error unmarshaling configuration schema.GroupVersionKind{Group:"kubeproxy.config.k8s.io", Version:"v1alpha1", Kind:"KubeProxyConfiguration"}: strict decoding error: unknown field "udpIdleTimeout"
[check-expiration] Error reading configuration from the Cluster. Falling back to default configuration

CERTIFICATE                EXPIRES                  RESIDUAL TIME   CERTIFICATE AUTHORITY   EXTERNALLY MANAGED
admin.conf                 Jul 13, 2024 01:51 UTC   364d            ca                      no
apiserver                  Jul 13, 2024 01:51 UTC   364d            ca                      no
apiserver-etcd-client      Jul 13, 2024 01:51 UTC   364d            etcd-ca                 no
apiserver-kubelet-client   Jul 13, 2024 01:51 UTC   364d            ca                      no
controller-manager.conf    Jul 13, 2024 01:51 UTC   364d            ca                      no
etcd-healthcheck-client    Jul 13, 2024 01:51 UTC   364d            etcd-ca                 no
etcd-peer                  Jul 13, 2024 01:51 UTC   364d            etcd-ca                 no
etcd-server                Jul 13, 2024 01:51 UTC   364d            etcd-ca                 no
front-proxy-client         Jul 13, 2024 01:51 UTC   364d            front-proxy-ca          no
scheduler.conf             Jul 13, 2024 01:51 UTC   364d            ca                      no

CERTIFICATE AUTHORITY   EXPIRES                  RESIDUAL TIME   EXTERNALLY MANAGED
ca                      Jul 07, 2032 11:45 UTC   8y              no
etcd-ca                 Jul 07, 2032 11:45 UTC   8y              no
front-proxy-ca          Jul 07, 2032 11:45 UTC   8y              no
```

It has been renewed successfully for another 364 days.

### Manage \~/.kube/config

Since the certificates were changed, we need to copy `/etc/kubernetes/admin.conf` to `~/.kube/config` again in order for us to use `kubectl` commands. This is due to the user not being logged in with the renewed certificates.

```bash
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Then `kubectl` commands will work properly.&#x20;

### Propagate config (optional)

Since the master node only has the updated config file, we might also want to propagate the config file to all worker nodes if we want to use `kubectl` in worker nodes.

```
scp /home/isu/.kube/config isu@172.20.41.152:/home/isu/.kube/
```

This will propagate `kube/config` to the worker node, change `/home/isu` and `172.20.41.152` to your username and worker nodes.

---
description: By Default Settings, noVNC might not work properly
---

# \[Nova] noVNC not working

When we make an instance in OpenStack, then we visit console, there might be some statement saying:

```
something went wrong,connection is closed
```

### 1. TroubleShooting

Check log from

```
/var/log/nova/nova-novncproxy.log
```

If it states something like:

```
20: connecting to: openstack-mag-worker-3:5900
handler exception: [Errno -2] No address found
```

This means host was not set properly.&#x20;

### 2. Fixing Hosts

In order to fix this, we need to manually set those entries to the `/etc/hosts` file:

```
172.25.244.136 openstack-mag-worker-3
172.25.244.115 openstack-mag-worker-1
```

Then check ping if the hostname was set properly


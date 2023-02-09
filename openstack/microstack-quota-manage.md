---
description: A cheat sheet for quota managmenet for Microstack.
---

# \[Microstack] Quota Manage

### Quota List

```
microstack.openstack quota show
```

### Set Quota

```
microstack.openstack quota set --name value tenet
```

For example, if you would like to set `cores` as 40 cores for project named `d52660ac5277409a8dedf40b2a9a119a`:

```
microstack.openstack quota set --cores 40 d52660ac5277409a8dedf40b2a9a119a
```

This will set the cores as 40 cores possible

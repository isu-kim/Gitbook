---
description: Overcommit hypervisor and allocate more resources.
---

# \[Microstack] Overcommit Hypervisor

Just like Kubernetes's resource policy (`requests` and `limits`), OpenStack offers us options to overcommit. You can achieve this by using `aggregate` option.

### Create Aggregation

```
microstack.openstack aggregate create extra-resources
```

This command will generate an `aggregation` named `extra-resources`.&#x20;

### Add Host to Aggregation

<pre><code><strong>microstack.openstack aggregate add host extra-resources openstack
</strong></code></pre>

This command will allow the `extra-resources` aggregation connected to host named `openstack`. Then you can check if it was successfully set or not by

```
microstack.openstack aggregate show extra-resources
```

An example output will be:

```
+-------------------+-----------------------------------------------------------------------------------------------------------+
| Field             | Value                                                                                                     |
+-------------------+-----------------------------------------------------------------------------------------------------------+
| availability_zone | None                                                                                                      |
| created_at        | 2023-02-15T04:20:05.000000                                                                                |
| deleted           | False                                                                                                     |
| deleted_at        | None                                                                                                      |
| hosts             | openstack                                                                                                 |
| id                | 1                                                                                                         |
| name              | extra-resources                                                                                           |
| properties        |                                                                                                           |
| updated_at        | None                                                                                                      |
+-------------------+-----------------------------------------------------------------------------------------------------------+
```

As you can see, the host `openstack` was set as aggregation for `extra-resources`.

### Set Property to Aggregation

```bash
openstack aggregate set --property cpu_allocation_ratio=2.0 extra-resources
```

You can apply 2x times vCPU than the physical CPU using `cpu_allocation_ratio`. There are a choice of options to use:

* `cpu_allocation_ratio`: This property sets the CPU allocation ratio for the hypervisor(s) in the aggregate. The CPU allocation ratio is the ratio of virtual CPU (vCPU) to physical CPU (pCPU) on the host. For example, setting `--property cpu_allocation_ratio=2.0` doubles the amount of virtual CPU available to instances running on the hypervisor(s) in the aggregate.
* `ram_allocation_ratio`: This property sets the RAM allocation ratio for the hypervisor(s) in the aggregate. The RAM allocation ratio is the ratio of virtual RAM to physical RAM on the host. For example, setting `--property ram_allocation_ratio=2.0` doubles the amount of virtual RAM available to instances running on the hypervisor(s) in the aggregate.
* `disk_allocation_ratio`: This property sets the disk allocation ratio for the hypervisor(s) in the aggregate. The disk allocation ratio is the ratio of virtual disk to physical disk on the host. For example, setting `--property disk_allocation_ratio=2.0` doubles the amount of virtual disk available to instances running on the hypervisor(s) in the aggregate.
* `availability_zone`: This property sets the availability zone for the hypervisor(s) in the aggregate. The availability zone is a logical partition of the OpenStack environment that separates resources and isolates failure domains. For example, setting `--property availability_zone=zone1` assigns the hypervisor(s) in the aggregate to the "zone1" availability zone.
* `pci_passthrough_whitelist`: This property sets a whitelist of PCI devices that are passed through to instances running on the hypervisor(s) in the aggregate. For example, setting `--property pci_passthrough_whitelist="{"vendor_id":"8086","product_id":"100e"}"` passes through PCI devices with a vendor ID of "8086" and a product ID of "100e" to instances running on the hypervisor(s) in the aggregate.

> Openstack recommends overcommiting CPU ration 16, and RAM ratio as 1.5 according to [her](https://docs.openstack.org/arch-design/design-compute/design-compute-overcommit.html)[e](https://docs.openstack.org/arch-design/design-compute/design-compute-overcommit.html).

### Creating Flavor for Aggregate

Also create `flavor` for aggregation by following command:

```
openstack flavor create --ram 8192 --disk 10 --vcpus 4 small-disk
```

Then apply aggregate `extra-resources` to `small-disk` using:

```
openstack aggregate set --property flavor=extra-large extra-resources
```

---
description: Allow access to instances using security group
---

# \[Microstack] Security Group

If your security group settings are not set correctly, you might have no access to some ports. For example, you can `ping` and `ssh` the instance, however are not able to use ports like `80.` This is due to security groups.&#x20;



### Get Security Group Info

```
microstack.openstack security group list
```

Get your security group information. This list you the total security groups.

```
+--------------------------------------+---------+------------------------+----------------------------------+------+
| ID                                   | Name    | Description            | Project                          | Tags |
+--------------------------------------+---------+------------------------+----------------------------------+------+
| 0e474f30-839b-4d32-a037-1440cef8966d | default | Default security group | 463a8828d5bb468c8b4b832c02d3a9c0 | []   |
| dd2f2ac3-d31c-4dac-bc13-069dd5f8c0c9 | default | Default security group | 2d79e3e32a1d4a1399061025a5071322 | []   |
+--------------------------------------+---------+------------------------+----------------------------------+------+
```

In here, find your `Project` id and the `ID` of the security group. (Since there will be some networks with same names `default`)

### Add Rules

You can add rules to security groups by following command. For protocols, use:

```
openstack security group rule create --proto icmp 0e474f30-839b-4d32-a037-1440cef8966d
```

This will enable your security group `ICMP` protocol access from outside of the instance.&#x20;

Also you can use following command to add TCP 80 access as well.

```
openstack security group rule create --proto tcp --dst-port 80 0e474f30-839b-4d32-a037-1440cef8966d
```

The example result will be like following

```
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field             | Value                                                                                                                                                   |
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| created_at        | 2023-02-14T08:21:04Z                                                                                                                                    |
| description       |                                                                                                                                                         |
| direction         | ingress                                                                                                                                                 |
| ether_type        | IPv4                                                                                                                                                    |
| id                | 929cc4fe-e190-4c4e-8269-ae17646cb5aa                                                                                                                    |
| location          | cloud='', project.domain_id=, project.domain_name='default', project.id='463a8828d5bb468c8b4b832c02d3a9c0', project.name='admin', region_name='', zone= |
| name              | None                                                                                                                                                    |
| port_range_max    | 80                                                                                                                                                      |
| port_range_min    | 80                                                                                                                                                      |
| project_id        | 463a8828d5bb468c8b4b832c02d3a9c0                                                                                                                        |
| protocol          | tcp                                                                                                                                                     |
| remote_group_id   | None                                                                                                                                                    |
| remote_ip_prefix  | 0.0.0.0/0                                                                                                                                               |
| revision_number   | 0                                                                                                                                                       |
| security_group_id | 0e474f30-839b-4d32-a037-1440cef8966d                                                                                                                    |
| tags              | []                                                                                                                                                      |
| updated_at        | 2023-02-14T08:21:04Z                                                                                                                                    |
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
```

### Validating

Once you have added 80 tcp to the security group rule, you can check it by running a `apache2` or `nginx` inside the instance. Then run

```
nc -zv instance_ip
```

If the group rule was set properly, it will be show like below:

```
Connection to 10.20.20.176 80 port [tcp/http] succeeded!
```

Now, you will have access to the port 80.

\

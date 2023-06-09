---
description: Add SRV Record to Bind9 DNS
---

# Bind9 SRV Record

Serving SRV record sometimes is necessary. For example, in Minecraft, if the server port does not end in 25565, then you shall add port number. This is quite bothering and SRV record can do that for you.&#x20;

### 1. Create Zone File

In order to add SRV Record to Bind9, you can first generate a zone file like this

```
$TTL 2d
$ORIGIN foo.kr.

@                               IN      SOA     ns1.foo.kr. ns1.foo.kr. (
                                                2023060100 ; serial
                                                12h        ; refresh
                                                15m        ; retry
                                                3w         ; expire
                                                2h         ;min ttl
                                                )
                                IN      NS      ns.foo.kr.

ns                              IN      A       123.123.123.123


; -- add dns records
```

This will first set up a SOA for the `foo.kr` zone. Also this will set up an A record with `ns.foo.kr` pointing `123.123.123.123`. So, the next thing you shall do is follow the steps below:

### 2. Add Records

In order for SRV record to work, you need an A record first. Then attach that A record to SRV record. An example is as it follows:

```
srv6.foo.kr.       IN      A       123.123.123.123
_minecraft._tcp.srv6.foo.kr.   IN      SRV     0 0 25599 srv6.foo.kr.
```

So this will basically make `srv6.foo.kr` point to `123.123.123.123:25599`. Therefore, you can access your (Minecraft) server with that `srv6.foo.kr` address.

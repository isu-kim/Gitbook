---
description: Get argument values of an event using Cilium's eBPF
---

# \[Kprobe] Get Argument Values

### 0. Background

When a Kprobe had a event happening, it will call a function with argument named `struct pt_regs *ctx` with this, we can know what the register values were using some eBPF functions.

> Let's assume that we were watching `kprobe/tcp_v4_rcv` to monitor TCP packet receive events.

### 1. Headers

For us to use `PT_REGS_PARM1`, `PT_REGS_RC` macros, we need to have following code:

```c
#include <bpf/bpf_tracing.h>
```

Since retrieving register values are different by architectures, we need to explictly define target architecture by:

```c
#define __TARGET_ARCH_x86
```

We are assuming that we have x86 system.&#x20;

> If you are using lots of `#include`s, including `vmlinux.h` , the target architecture definition **must** before any `#include` statements. For example, it shall be like below:
>
> ```c
> #define __TARGET_ARCH_x86
> #include "vmlinux.h"
> #include <bpf/bpf_helpers.h>
> #include <bpf/bpf_core_read.h>
> #include <bpf/bpf_tracing.h>
> ```

> If you would like to see the list of available architectures, check [https://elixir.bootlin.com/linux/latest/source/tools/lib/bpf/bpf\_tracing.h](https://elixir.bootlin.com/linux/latest/source/tools/lib/bpf/bpf\_tracing.h) for more information on available architectures and their names.

### 2. Accessing ctx

We can retrieve the first parameter value using following statement:

```c
struct sk_buff *skb = (struct sk_buff *)PT_REGS_PARM1(ctx);
```

Since `tcp_v4_rcv` requires `struct sk_buff`'s pointer as first argument, this will retrieve the data.



### 3. Accessing ctx

Since we have `struct sk_buff *skb` which is a pointer that points to a struct, we might fall into a temptation to use

```c
u32 length = skb->len; // To retrieve packet length
```

But, this will return error something looks similar to the following message:

```
load program: permission denied: 19: (61) r1 = *(u32 *)(r9 +112): R9 invalid mem access 'inv' 
```

We need to use `bpf_probe_read` in order to read the values.

```c
u32 len = 0;
bpf_probe_read(&len, sizeof(len), &skb->len);
```

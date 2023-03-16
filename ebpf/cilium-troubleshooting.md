---
description: Trouble Shooting Guides for eBPF by Cilium
---

# \[Cilium] TroubleShooting

## 1. Compiling

### 1.1 Clang

#### 1.1.1 'asm/types.h' file not found

**Example Command:**

```
clang -O2 -target bpf -c syscall_monitor.c -o syscall_monitor.o
```

**Solution:**

```
sudo apt-get install gcc-multilib
```

#### 1.1.2 Dependency Problems

**Example Error Message:**

```
... incomplete definition of type 'struct task_struct'
```

**Solution:**

Use `vmlinux.h` for solving dependency issues with C code. The commands came from [https://www.grant.pizza/blog/vmlinux-header/](https://www.grant.pizza/blog/vmlinux-header/)&#x20;

```
sudo apt-get install linux-tools-common
sudo apt-get install linux-tools-5.15.0-67-generic # Check your Linux version
bpftool btf dump file /sys/kernel/btf/vmlinux format c > vmlinux.h
```

Then `#include vmlinux.h` instead of the dependency that you are using.

#### 1.1.3 Top Level Declarator Error&#x20;

**Example Error Message:**

```
error: expected ';' after top level declarator
char __license[] SEC("license") = "MIT";
```

**Solution:**

```
#include <linux/bpf.h>
```

#### 1.1.4 'bpf/\*.h' file not found

**Example Error Message:**

```
fatal error: 'bpf/bpf_helpers.h' file not found
#include <bpf/bpf_helpers.h>
```

**Solution:**

```
sudo apt-get install libbpf-dev
```


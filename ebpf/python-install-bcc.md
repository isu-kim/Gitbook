---
description: Install Python3 bcc
---

# \[Python] Install BCC

Installing bcc for Python3 is kind of tricky. If it was installed not correctly, this will throw following error:

```python
ImportError: cannot import name 'BPF' from 'bcc'
```

This post will help you install bcc for Python3

### Install bfcc

```bash
apt install python3-bpfcc 
```

### Uninstall bcc

If you have installed `pip install bcc` before, uninstall it by

```
pip uninstall bcc
```

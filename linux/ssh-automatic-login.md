---
description: SSH Automatic Login using Keys
---

# SSH Automatic Login

### On Your PC

```
ssh-keygen
```

### On SSH Server

```
mkdir -p ~/.ssh
echo PUB_KEY_STRING >> ~/.ssh/authorized_keys
chmod -R go= ~/.ssh
chown -R username ~/.ssh
```

Replace `PUB_KEY_STRING` to the content that was in your `~/.ssh/id_rsa.pub`

### Oneliner - On Your PC

```
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

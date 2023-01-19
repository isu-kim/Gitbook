---
description: Nginx HTTPS using Letsencrypt
---

# Nginx HTTPS

Purchasing SSLs are costly. Therefore, we will be using Letsencrypt for making our Nginx web server able to use HTTPS.

### Generate SSL

```
sudo apt-get install letsencrypt
sudo certbot certonly
```

### Edit Nginx Config

In file `/etc/nginx/conf.d/default.conf`

```
server {
        listen 443 ssl;
        ssl_certificate /etc/letsencrypt/live/DOMAIN_NAME/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/DOMAIN_NAME/privkey.pem;
}

server {
        listen 80 default_server;
        return 301 https://$host$request_uri;
        return 404;
}
```

> Set `DOMAIN_NAME` as your domain name that you have generated SSL with. The config above will also redirect HTTP to HTTPS as well

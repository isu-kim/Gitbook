---
description: Make Users Login for a Specific Endpoint
---

# Nginx Login Page

Sometimes, there are cases when you would like to set auth for a specific endpoint. This is for that purpose.



### Install Package

```
sudo apt-get update
sudo apt-get install apache2-utils
```

### Make User

```
sudo htpasswd -c /etc/nginx/.htpasswd sammy
```

### Edit Nginx

Now, edit files in `/etc/nginx/conf.d/` and add following two lines to the location that you would like protection using `/etc/nginx/.htpasswd`.

```
auth_basic "Restricted Content";
auth_basic_user_file /etc/nginx/.htpasswd;
```

Example:

```
server {
	listen 80 default_server;

	location /secret/ {
		root /var/web;
		auth_basic "Restricted Content";
        	auth_basic_user_file /etc/nginx/.htpasswd;
	}

}
```

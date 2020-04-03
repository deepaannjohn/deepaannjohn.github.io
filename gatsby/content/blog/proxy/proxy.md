---
title: Proxy
date: "2020-04-03T22:12:03.284Z"
description: "Proxy basics"
---

**How to set up a forward proxy?**

**nginx as forward proxy - did not work with authentication**

I tried to set up nginx as forward proxy on my Windows machine and it did not work. Well, it did work when the proxy was not configured for authentication. 

This was how I set it up.

I used a docker image reiz/nginx_proxy as I read that nginx docker image does not work for https sites.

```
docker run -d -p 8888:8888 -v /d:/misc/dockerfiles/nginx/conf/nginx_whitelist.conf:/usr/local/nginx/conf/nginx.conf reiz/nginx_proxy

```
This proxy works for both https and https sites.

Then I set up authentication for the proxy:

```
docker run -d -p 8888:8888 -v /d:/misc/dockerfiles/nginx/conf/nginx_whitelist.conf:/usr/local/nginx/conf/nginx.conf -v /d:/misc/dockerfiles/xampp/apache/bin/.htpasswd:/var/passwd/nginx/.htpasswd reiz/nginx_proxy

```
config file
```
user www-data;
worker_processes auto;
daemon off; # Don't run Nginx as daemon, as we run it in Docker we need a foreground process.
events { }


http {
    server_names_hash_bucket_size 128;

    access_log /var/log/nginx_access.log;
    error_log /var/log/nginx_errors.log;

    # Whitelist All
    server {
        listen       8888;
	server_name   ~.;
	auth_basic "Restricted";
        auth_basic_user_file /var/passwd/nginx/.htpasswd;
        proxy_connect;
        proxy_max_temp_file_size 0;
        resolver 8.8.8.8;
        location / {
           proxy_pass http://$http_host;
           proxy_set_header Host $http_host;
        }
    }

    # Everything else is denied
    server {
        listen       8888;
        return 404;
    }

}
```

There are online tool generators for generating .htpasswd file.

**Squid forward proxy**

It was very easy to set up Squid on my Windows machine.

I followed the instructions below for setting up a basic http forward proxy sans autehntication.

<https://www.youtube.com/watch?v=bSHCiZq2TrU>

Then followed the instructions below to set up authentication:

https://gist.github.com/yvanin/ef831720112c1f6ee8c3

```
auth_param basic program "/lib/squid/basic_ncsa_auth.exe" "/etc/squid/.htpasswd"
acl ncsa_users proxy_auth REQUIRED
http_access allow ncsa_users

```

**HTTPS Proxy**

Setting up an https proxy looked not so easy on Squid. Hence, I tested my application just using http proxy and hope it would work just find with HTTP proxy.
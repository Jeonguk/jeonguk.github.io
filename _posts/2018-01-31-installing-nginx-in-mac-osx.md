---
layout: post
title:  "Installing Nginx in Mac OS X"
date:   2018-01-31 16:32:10 +0700
categories: [server]
---

# Installing Nginx in Mac OS X

### Install with brew

```
$ brew update
```

```
$ brew install nginx
```

* RUN

```
$ sudo nginx
```

* Testing

```
http://localhost:8080/
```

* Congiguration (default place)

```
/usr/local/etc/nginx/nginx.conf
```

* nginx server stop

```
$ sudo nginx -s stop
```

* default port (8080) change -> 80

* change

```
 server {
listen       8080;
server_name  localhost;

#access_log  logs/host.access.log  main;

location / {
    root   html;
    index  index.html index.htm;
}
```

* to

```
server {
listen       80;
server_name  localhost;

#access_log  logs/host.access.log  main;

location / {
    root   html;
    index  index.html index.htm;
}
```

* relaunch nginx

```
sudo nginx
```

* nginx html folder (brew install only)

```
/usr/local/Cellar/nginx/1.12.2_1/html
```

* defualt path configuration change

```
server {
listen       80;
server_name  localhost;

#access_log  logs/host.access.log  main;

location / {
    root   html;
    index  index.html index.htm;
}
```

* to

```
 server {
listen       80;
server_name  localhost;

#access_log  logs/host.access.log  main;

location / {
    root   /Users/to/www;
    index  index.html index.htm;
}
```


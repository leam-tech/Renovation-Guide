---
description: >-
  A brief guide on how to setup nginx for renovation-cms to use with frappe
  backend
---

# Nginx Configuration

## Basic Configuration

Suppose, you have a site called `example.com`, with renovation-cms on `cms.example.com` & frappe backend on `frappe.example.com`. We will have `cms.example.com/api` reverse proxied to frappe backend to prevent `CORS` issue.

{% hint style="info" %}
The production build of `renovation-cms` has backend url set to `/api`
{% endhint %}

```bash
upstream frappe-server {
	server 192.168.0.101;
	keepalive 16;
}

map $http_host $frappe_site_ifisd {
	"cms.example.com" "frappe.example.com";
	"" $http_host; 
}

# LTS-Renovation
server {
	listen 443 ssl;
	server_name
		cms.example.com;
	
	root /home/frappe/www/lts-renovation/dist;
	
	ssl_certificate		/home/frappe/.ssl/example-cert.pem;
	ssl_certificate_key	/home/frappe/.ssl/example-key.pem;

	location / {
		try_files $uri $uri/ /index.html;
	}
	
	location /api {
		rewrite /api/(.*) /$1  break;
		proxy_pass 					http://frappe-server;
		proxy_set_header 		Host $frappe_site_ifisd1;
		proxy_set_header 		Accept application/json;
		proxy_redirect 			off;
		proxy_buffering 		off;

		# include details about the original request
		proxy_set_header X-Original-Host $http_host;
		proxy_set_header X-Original-Scheme $scheme;
		proxy_set_header X-Forwarded-For $remote_addr;

		# Websocket support
		proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

	}
	
	# optimizations
	sendfile on;
	keepalive_timeout 15;
	client_max_body_size 50m;
	client_body_buffer_size 16K;
	client_header_buffer_size 1k;

	# enable gzip compresion
	# based on https://mattstauffer.co/blog/enabling-gzip-on-nginx-servers-including-laravel-forge
	gzip on;
	gzip_http_version 1.1;
	gzip_comp_level 5;
	gzip_min_length 256;
	gzip_proxied any;
	gzip_vary on;
	gzip_types
		application/atom+xml
		application/javascript
		application/json
		application/rss+xml
		application/vnd.ms-fontobject
		application/x-font-ttf
		application/font-woff
		application/x-web-app-manifest+json
		application/xhtml+xml
		application/xml
		font/opentype
		image/svg+xml
		image/x-icon
		text/css
		text/plain
		text/x-component
		;
		# text/html is always compressed by HttpGzipModule
}
```


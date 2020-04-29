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

Now, lets break it down.

#### Upstream

Here you can specify the ip address of the VPS where frappe is hosted. If it is hosted on the same VPS, have it replaced with `server localhost;`

```bash
upstream frappe-server {
	server 192.168.0.101;
	keepalive 16;
}
```

#### The Server Block

You can find commong nginx directives inside this. The location block `location /` is set just like how we set for a SPA, and other nginx optimizations like `sendfile` and `gzip` compression has been enabled, along with basic `SSL` setup.

#### The API Location Block

One of the important section of this configuration is maybe the `/api` location block. It forwards all the requests coming in at this endpoint to the frappe server. Every frappe requests, be it downloads, `SocketIO` or be it anything that concerns frappe, it goes through here. Please follow the comments specified for deeper understanding.

```bash
	location /api {
		# rewrite will change all /api/<frappe-url> to just /<frappe-url>
		# when it gets forwarded to the upstream server
		rewrite /api/(.*) /$1  break;
		
		# the directive asking nginx to forward the traffic
		proxy_pass 					http://frappe-server;
		
		# This helps in selecting the specific site in a multi-site bench
		# More on this later
		proxy_set_header 		Host $frappe_site_ifisd1;
		
		# We expect only JSON from frappe-py
		proxy_set_header 		Accept application/json;
		
		# Lets the http-301/302 received from frappe be left intact
		proxy_redirect 			off;
		
		# Forward the response received from the frappe server without buffering it
		# Locally. This helps in huge file downloads. It forwards the data obtained
		# from client synchronously
		proxy_buffering 		off;

		# include details about the original request
		proxy_set_header X-Original-Host $http_host;
		proxy_set_header X-Original-Scheme $scheme;
		proxy_set_header X-Forwarded-For $remote_addr;

		# Websocket support (SocketIO)
		proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

	}
```

#### Mapping Renovation-CMS Host to Frappe-Site in Multi Site Bench

| RENOVATION-CMS HOST | FRAPPE SITE NAME |
| :--- | :--- |
| cms.example-A.com | frappe.example-A.com |
| cms.example-B.com | frappe.example-B.com |
| ... | ... |

Take the case of a frappe-bench hosting 10 frappe sites. And there are independent renovation-cms hosts for the same, as shown in the table above. We make use of `nginx-map`directive to help us with this situation.

As we forward all the request coming in at `/api`, we manually set the `Host` header that gets forwarded with the value of the variable `frappe_site_ifisd1`\(ignore the random part, it is just to ensure that no nginx variable name conflict happens\) which is dependent on `$http_host` which is the incoming Host header \(cms host\)

```bash
location /api {
    ...
    proxy_set_header Host $frappe_site_ifisd1;
    ...
}
```

And look at the map defined at the top of the configuration file.

```bash
map $http_host $frappe_site_ifisd {
	"cms.example-A.com" "frappe.example-A.com";
	"cms.example-B.com" "frappe.example-B.com";
	"" $http_host; 
}
```

Please checkout `nginx map` documentations for more details.

#### Great! You stayed till the end!

Please reach out to us if you find yourself in trouble with configurations :\)


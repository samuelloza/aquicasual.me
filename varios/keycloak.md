---
title: Jugando con Keycloack
description: Recientemente conoci a Keycloack es un gestor de usuarios por asi llamarlo
published: true
date: 2020-07-02T00:39:05.047Z
tags: keycloack
editor: markdown
---

# Jugando con Keycloack





1. Levantando Keycloack en Docker 

```
docker run -d -p 8080:8080 -e KEYCLOAK_USER=keycloak -e KEYCLOAK_PASSWORD=mi_clave_segura -e PROXY_ADDRESS_FORWARDING=true quay.io/keycloak/keycloak:10.0.2
```

2. Levantando nginx proxy inverso 

```bash
server {
	listen 443;

	server_name auth.misito.com;

	# SSL
	ssl_certificate /etc/letsencrypt/live/auth.misito.com/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/auth.misito.com/privkey.pem;
  error_log  /var/log/nginx/auth.misito.com.log error;
 
 location ~ /(.*) {
     proxy_pass          http://127.0.0.1:8080;
     proxy_set_header    Host               $host;
     proxy_set_header    X-Real-IP          $remote_addr;
     proxy_set_header    X-Forwarded-For    $proxy_add_x_forwarded_for;
     proxy_set_header    X-Forwarded-Host   $host;
     proxy_set_header    X-Forwarded-Server $host;
     proxy_set_header    X-Forwarded-Port   $server_port;
     proxy_set_header    X-Forwarded-Proto  $scheme;
 }
 include nginxconfig.io/general.conf;
}

# HTTP redirect
server {
	listen 80;
	server_name auth.misito.com;
	include nginxconfig.io/letsencrypt.conf;
	location / {
		return 301 https://auth.misito.com;$request_uri;
	}
}
```

En construcción 

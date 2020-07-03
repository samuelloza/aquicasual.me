---
title: Migrando contenedores
description: 
published: true
date: 2020-07-03T23:44:47.391Z
tags: servers
editor: markdown
---

# Migrando contenedores


Les comparto esta vitacora que me hice para migrar los contenedores de un ser servidor (Gcloud) a otro.

Primero lo primero un inventario de lo que se tiene corriendo,

Revisamos los contenedores que estan corriendo
```
docker ps
```

```
CONTAINER ID        IMAGE                                                         COMMAND                  CREATED             STATUS              PORTS                  NAMES
7e49b8eb5ab8        registry.gitlab.com/repo/front   "/docker-entrypoint.…"   3 months ago      Up 3 months       0.0.0.0:8081->80/tcp   name_container
...
```
Revisamos las configuraciones del nginx
```
ls -l /etc/nginx/conf.d/
```

Hasta este punto ya conocemos los contenedores que estan corriendo y las configuraciones del nginx.

1. Ingresando por primera vez al vps nuevo

Lo primero que tenemos que hacer es actualizar el sistema elegi un **Centos 8**

````bash
yum update
````
Ahora instalamos docker
```bash
 curl -fsSL https://get.docker.com -o get-docker.sh
 sh get-docker.sh
```
Ups un error
```txt
Error:
 Problem: package docker-ce-3:19.03.8-3.el7.x86_64 requires containerd.io >= 1.2.2-3, but none of the providers can be installed
  - cannot install the best candidate for the job
  - package containerd.io-1.2.10-3.2.el7.x86_64 is excluded
  - package containerd.io-1.2.13-3.1.el7.x86_64 is excluded
  - package containerd.io-1.2.2-3.3.el7.x86_64 is excluded
  - package containerd.io-1.2.2-3.el7.x86_64 is excluded
  - package containerd.io-1.2.4-3.1.el7.x86_64 is excluded
  - package containerd.io-1.2.5-3.1.el7.x86_64 is excluded
  - package containerd.io-1.2.6-3.3.el7.x86_64 is excluded
```
Aqui la solucion
```bash
yum install -y https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
```
```bash
yum install docker-ce
```
```bash
docker -v
Docker version 19.03.12, build 48a66213fe
```
Habilitando el daemon de nginx
```bash
systemctl enable nginx
```

Iniciando docker
```bash
systemctl start docker
```


![metalslug-1.png](/memes/metalslug-1.png)


## Ahora el turno de **nginx**

> ¿Podemos usar nginx desde docker ? 
> Si
> Pero no lo usare
{.is-info}

Instalamos nginx
```bash
yum install nginx
```
> Tool para nginx
> https://www.digitalocean.com/community/tools/nginx
>  
{.is-info}

dentro el directorio de configuracion de nginx agregamos el nuevo sitio
```bash
vi /etc/nginx/conf.d/mi-sitio.com.conf
```

```
server {
	server_name mi-sitio.com www.mi-sitio.com beta.mi-sitio.com;
	index index.html index.htm index.nginx-debian.html;

    access_log /var/log/nginx/mi-sitio.com.access.log;
    error_log /var/log/nginx/mi-sitio.com.error.log;

	location / {
	    proxy_set_header Host               $host;
            proxy_set_header            X-Real-IP          $remote_addr;
            proxy_set_header            X-Forwarded-For    $proxy_add_x_forwarded_for;
            proxy_set_header            X-Forwarded-Proto  $scheme;
            proxy_redirect              off;
            proxy_read_timeout          1m;
            proxy_connect_timeout       1m;
            proxy_pass                  http://127.0.0.1:8081;
	}

    listen [::]:443 ssl ipv6only=on;
    listen 443 ssl;
    ssl_certificate /etc/letsencrypt/live/mi-sitio.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/mi-sitio.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
}

server {
	listen 80;
	server_name mi-sitio.com;

    if ($host = mi-sitio.com) {
        return 301 https://$host$request_uri;
    }

    return 404;
}
```

Agregamos las configuraciones para letsencrypt
```
mkdir -p /etc/letsencrypt/
vi /etc/letsencrypt/options-ssl-nginx.conf
```
Con el siguiente contenido
```
# This file contains important security parameters. If you modify this file
# manually, Certbot will be unable to automatically provide future security
# updates. Instead, Certbot will print and log an error message with a path to
# the up-to-date file that you will need to refer to when manually updating
# this file.

ssl_session_cache shared:le_nginx_SSL:1m;
ssl_session_timeout 1440m;

ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;

ssl_ciphers "ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS";
```

Para el ejemplo usare Cloudflare

> https://support.cloudflare.com/hc/es-es/articles/200167836-Administrar-tokens-y-claves-API
{.is-info}

![metalslug-2.png](/memes/metalslug-2.png)



## Letsencrypt con docker
Creamos una directorio secrets donde estara el token para ingresar a cloudflare.

```
mkdir -p /root/secrets/
```
Del paso anterior tenemos nuestro token, ahora lo usaremos
```
vi /root/secrets/.cloudflare.ini
```
con el siguiente contenido
```
dns_cloudflare_email = tu-corre@correo.com
dns_cloudflare_api_key = tu-token
```
y le damos permisos 
```
chmod 600 /root/secrets/.cloudflare.ini
```
Ahora creamos un archivo **ssl.sh**, donde esta el script para obtener los certificados

```
vi /opt/ssl.sh
```
```
#!/bin/bash
docker run -ti --rm \
-v "/etc/letsencrypt:/etc/letsencrypt" \
-v "/home/sloza/bso-ssl/.cloudflare.ini:/cloudflare.ini" \
certbot/dns-cloudflare:latest \
certonly \
--dns-cloudflare \
--dns-cloudflare-credentials /cloudflare.ini \
-d "mi-sitio.com" \
-d "*.mi-sitio.com" \
--email tu-correo@correo.com \
--agree-tos \
--server https://acme-v02.api.letsencrypt.org/directory
```
Recuerda cambiar estas lineas por tus datos

> -d "mi-sitio.com" \
> -d "*.mi-sitio.com" \
> --email tu-correo@correo.com \
> 
{.is-warning}


Si todo salio bien, podremos ver la siguiente pantalla


![ssl_ok.png](/ssl_ok.png)


![metalslug-3.png](/memes/metalslug-3.png)


# Pasos finales
Ahora iniciamos el contenedor docker
```
docker run -d -p 8081:80 --name nombre-contenedor --restart=always registry.gitlab.com/registry.gitlab.com/repo/front
```


Nos dirigimos a CloudFlare y agregamos un nuevo **registro de tipo A**

![cloudflare_type_a.png](/cloudflare/cloudflare_type_a.png)

Agregamos una tarea crontab para que se actualice los certificados automaticamente
```
*  *    1 */1 * root   /opt/ssl.sh
```

> 
> Tienes dudas sobre crontab ? 
> Mira este sitio
> https://crontab.guru/
> 
> 
{.is-info}

Entra a beta.mi-sito.com 


![image.png](/memes/image.png)


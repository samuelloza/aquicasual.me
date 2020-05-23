---
title: 2. Instalacion
description: Instalacion
published: true
date: 2020-05-23T15:42:28.856Z
tags: docker
---

# 2. Instalación

	
### Pre requisitos
No se necesitan habilidades especificas para este tutorial, un básico conocimiento sobre la terminal y usar un editor de texto. Previa experiencia en desarrollo web sera útil, pero no es un requisito. A medida que avancemos en este tutorial, utilizaremos [Docker Hub](https://hub.docker.com/).

### Configuraciones del Equipo

Instalar todas las herramientas en tu computadora puede ser una tarea desalentadora, pero instalar Docker en tu sistema operativo es una tarea muy facil.

La *guia de inicio* de Docker tiene detalladas instrucciones para instalar Docker en:
- [Mac](https://docs.docker.com/docker-for-mac/), 
- [Linux](https://docs.docker.com/engine/installation/linux/) 
- [Windows](https://docs.docker.com/docker-for-windows/).

*Si usas Docker para Windows* asegúrate de tener [shared your drive](https://docs.docker.com/docker-for-windows/#shared-drives).

*Nota Importante* Si estas usando una versión antigua de Windows o MacOS es posible que necesites usar [Docker Machine](https://docs.docker.com/machine/overview/).

*Los comandos funcionan en  bash o Powershell en Windows*

Una vez instalado Docker, prueba la instalación ejecutando la siguiente linea:
```bash
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
03f4658f8b78: Pull complete
a3ed95caeb02: Pull complete
Digest: sha256:8be990ef2aeb16dbcb9271ddfe2610fa6658d13f6dfb8bc72074cc1ca36966a7
Status: Downloaded newer image for hello-world:latest

Hello from Docker.
This message shows that your installation appears to be working correctly.
...
```
### Tip

Usa estos comandos con tu **usuario**
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
chmod +x get-docker.sh
sudo ./get-docker.sh
sudo usermod -aG docker $USER
```




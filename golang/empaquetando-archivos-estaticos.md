---
title: Empaquetando archivos estaticos
description: Empaquetando archivos estaticos
published: true
date: 2020-07-01T20:22:36.399Z
tags: golang
editor: markdown
---

# Empaquetando archivos estáticos

:) Hola :)

En esta entrada veremos como empaquetar archivos dentro de un binario,
para el ejemplo usaré una página simple en html.

## Pasos

1. Crear el contenido de la página web

```bash
mkdir public
cd public
```
Dentro del directorio agregaremos un html simple
```bash
nano index.html
```
Con el siguiente contenido
```html
<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8">
</head>
<body>
<h1> Hola Mundo </h1>
</body>
</html>
```
2. Creando los archivos go

```bash
cd ..
nano main.go
```
Con el siguiente contenido
```go
package main

import (
"log"
"net/http"

"github.com/gobuffalo/packr/v2"
)

func main() {
box := packr.New("public", "./public")
server := http.NewServeMux()

server.Handle("/", http.FileServer(box))
port := "8082"
log.Println("http://localhost:" + port)
http.ListenAndServe(":"+port, server)
}
```
y el archivo go.mod
```go
go init demo
```
La estructura del proyecto quedará como:
```
.
├── go.sum
├── main.go
└── public
└── index.html
```
3. Probar que todo esté en orden

```go
go get -u github.com/gobuffalo/packr/v2/packr2
go run ./main.go
```
Si todo salio bien, la salida será similar a esta:
```
2020/07/01 13:48:06 http://localhost:8082
```
y abra el navegador localhost:8082
![image.png](/image.png)

ehh, hasta el momento está funcionando

4. Construyendo el empaquetado

```bash
packr2 build -o salida main.go
packr2 clean
```
En otra terminal ejecutamos el binario construido

```bash
./salida
```
![packr2.png](/golang-empaquetado/packr2.png)

y listo ya tenemos el binario y el html empaquetado

# Referencias
https://github.com/gobuffalo/packr
https://www.ueber.net/who/mjl/blog/p/assets-in-go-binaries/ (me pareció que esta solución es muy interesante)
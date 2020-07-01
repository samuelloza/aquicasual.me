---
title: Empaquetando archivos estaticos
description: Empaquetando archivos html y js dentro deun binario de Go
published: true
date: 2020-07-01T17:41:53.509Z
tags: golang
editor: markdown
---

# Empaquetando archivos estaticos

Hola, en esta entrada veremos como empaquetar archivos estaticos, 
para el ejemplo me construi una aplicacion con html y javascript simple.

La idea es empaquetar los archivos html y javascript dentro de un binario de Go.

Primero crearemos un directorio 
```bash
mkdir public
cd public
```
Dentro del directorio agregaremos un html y javascript simple

nano index.html

con el siguiente contenido
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




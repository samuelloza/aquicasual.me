---
title: Apuntes Golang
description: 
published: true
date: 2023-08-17T17:44:01.714Z
tags: golang
editor: markdown
dateCreated: 2023-08-17T17:43:59.211Z
---

# Apuntes Golang


### Instalacion

Yo uso Manjaro
```bash=
yay -S go
```





## defer

Cuando se termina de ejecutar el programa se ejecuta la funcion defer

```go=
package main

import (
	"fmt"
)

func main(){
	a:= "hola"
	b:= 10
	defer sayMundo()

	if b > 5 {
	  return
	}
  
	fmt.Println(a)
}

func sayMundo(){
	fmt.Println("mundo")
}
```

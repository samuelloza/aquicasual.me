---
title: Apuntes Golang
description: 
published: true
date: 2020-05-23T17:10:59.492Z
tags: golang
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

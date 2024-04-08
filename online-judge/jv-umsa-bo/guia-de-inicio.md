---
title: Guía de inicio 
description: 
published: true
date: 2024-04-08T13:37:25.365Z
tags: 
editor: markdown
dateCreated: 2024-02-14T19:32:52.353Z
---

# Primeros Pasos con el Juez Virtual

Para comenzar, diríjase a la página del juez virtual, cuya dirección es [https://jv.umsa.bo](https://jv.umsa.bo).

![Imagen del juez virtual](/patito/image.png)

## Registro de Usuario

En la parte superior derecha, encontrará el botón de **Registro**.

![Imagen del registro](/patito/registro.png)

En esta página, usted podrá registrarse. Recuerde tener un correo electrónico válido; se recomienda usar un correo personal. Recuerde que su nombre de usuario es su *nickname*.

## Inicio de Sesión

En la parte superior derecha, se encuentra el botón de **Iniciar sesión**.

![Imagen de inicio de sesión](/patito/image.png)

A continuación, proporcione sus datos: el nombre de usuario (*nickname*) y su contraseña.

![Imagen de login](/patito/login.png)

## Envío de Problemas

Para poder ver los problemas que se encuentran en el juez virtual, haga click en **Problemas** y seleccione un problema.

![Imagen de problemas](/patito/problems.png)

Para el ejemplo, seleccionaremos el problema **1000 A+B**.

![Imagen del problema A+B](/patito/a+b.png)

Cada problema tiene las siguientes partes:

- **Descripción**: Describe el enunciado del problema.
- **Entrada**: Describe la forma en la que están los datos de entrada.
- **Salida**: Describe cómo se espera que presente la solución del problema.
- **Ejemplo de entrada**: Este es un ejemplo de cómo son sus datos de entrada.
- **Ejemplo de salida**: Esta es la solución en el formato esperado para los datos de entrada.

Para enviar una solución, primero se debe escoger el lenguaje de programación, que se encuentra al centro de la pantalla. En el caso de Java, la clase debe tener el nombre de **Main** y el método principal debe llamarse `main`. Es importante recalcar que el programa debe ser un solo archivo.

![Imagen de envío](/patito/submit.png)

A continuación, se redirigirá a la pantalla de estado.

![Imagen de estado](/patito/status.png)

Las respuestas del juez pueden incluir:

- **Pending**: Indica que aún está pendiente de revisión.
- **Pending Rejudge**: Significa que se ha decidido volver a evaluar los envíos de un problema específico.
- **Compiling**: Indica que el programa está siendo compilado.
- **Running & Rejudging**: Indica que el programa está siendo procesado y reevaluado.
- **Accepted**: La solución que enviamos es correcta y ha sido aceptada.
- **Presentation Error**: Se presenta cuando hay espacios adicionales en la respuesta.
- **Wrong Answer**: Indica que los resultados generados por el programa son diferentes a los esperados.
- **Time Limit Exceeded**: Se da cuando el tiempo de proceso es mayor al esperado.
- **Memory Limit Exceeded**: Los programas generalmente deben resolverse utilizando un límite específico de memoria RAM.
- **Output Limit Exceeded**: Cuando el archivo de salida es mayor que el permitido.
- **Compile Error**: Significa error de compilación.

Para más información, puede revisar [FAQs del juez virtual](https://jv.umsa.bo/oj/faqs.php).

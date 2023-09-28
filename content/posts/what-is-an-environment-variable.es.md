---
title: "¿Qué es una Variable de Entorno?"
slug: "que-es-una-variable-de-entorno"
description: "¿Para qué sirve una variable de entorno? ¿Cómo se usa? Todo en un minuto."
summary: "¿Para qué sirve una variable de entorno? ¿Cómo se usa? Todo en un minuto."
date: 2023-09-18T20:12:22+02:00
draft: false
tags: ["mini pildoras"]
---

Una variable de entorno es un dato global, almacenado en el sistema operativo, que puede ser leido fácilmente por cualquier programa.

Habitualmente lo utilizarás para gestionar datos que cambian entre un entorno de 'producción', y un entorno de 'desarrollo'. De tal modo que las variables de entorno te permiten cambiar el comportamiento de tu programa, sin necesidad de cambiar el código.

## ¿Qué datos se guardan en las Variables de Entorno?

Realmente puedes guardar cualquier tipo de dato primitivo (los aprendiste en [el post de las variables](/es/posts/que-es-una-variable/)), de una manera tan sencilla como esta:

```text
MY_NAME=Arturo
YT_CHANNEL_URL=https://www.youtube.com/@arturosource
MAX_TIMEOUT=10
```

Generalmente, la variable de entorno la escribirás en mayúscula, seguida del nombre de la variable un `=`, y por último, el valor sin espacios al principio ni al final. Pero existen varias maneras de hacer saber a un programa las variables que tienen que usar. Me centraré en Linux, que es donde comúnmente programarás, y desplegarás tus programas.

1. A nivel de sistema operativo: es tan sencillo como editar el archivo `/etc/environment`, y sigue el estilo que aparece anteriormente. Utiliza el editor de texto `nano`, por ejemplo, y necesitarás permisos para editar el archivo. Así que el comando será `sudo nano /etc/environment`.
2. A nivel de programa: si ejecutas una aplicación desde la terminal, puedes asignar tantas variables como quieras antes de la ejecución. Usando las del ejemplo anterior, podrías escribir un comando así `MY_NAME=Arturo MAX_TIMEOUT=10 bash ./coolbashscript`.
3. En un archivo que leerá tu aplicación: en los lenguajes modernos tendrás alguna librería con la que puedes leer un fichero como si tuviera variables de entorno, comúnmente se llamará `.env`. Tendrás que buscar la librería correspondiente de tu lenguaje de programación, y listo.

### ¿Qué datos es más común almacenar?

Almacena cualquier dato que creas pertinente, con la experiencia sabrás cuáles hay que almacenar y cuáles no. Pero aquí tienes algunos ejemplos para empezar.

- **Rutas del sistema**: Indica a un programa dónde buscar archivos ejecutables, bibliotecas, recursos, o archivos de configuración.
- **Idioma y región**: Determina la configuración regional y de idioma utilizada por tu aplicación.
- **Datos de autenticación**: Almacena credenciales de autenticación, como claves de API o contraseñas.
- **Configuración**: Personaliza campos como el puerto de escucha, el nombre de dominio, o el nivel de depuración.

## ¿Cómo usar variables de entorno en tu programa?

Para finalizar, algunos ejemplos de cómo se leen variables de entorno en los lenguajes más comunes.

Con Golang, importando `os`.

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    ytChannel := os.Getenv("YT_CHANNEL_URL")
    fmt.Printf("Valor de YT_CHANNEL_URL: %s\n", ytChannel)
}
```

Con JavaScript, usando la variable global `process`.

```js
const ytChannel = process.env.YT_CHANNEL_URL;
console.log(`Valor de YT_CHANNEL_URL: ${ytChannel}`);
```

Con PHP, la función global `getenv`.

```php
$ytChannel = getenv("YT_CHANNEL_URL");
echo "Valor de YT_CHANNEL_URL: " . $ytChannel;
```

Con Python, impostando `os` también.

```python
import os

ytChannel = os.getenv("YT_CHANNEL_URL")
print(f"Valor de YT_CHANNEL_URL: {ytChannel}")
```

---
title: "¿Qué es una API?"
slug: "que-es-una-api"
description: ""
summary: ""
date: 2023-09-20T19:19:28+02:00
draft: false
tags: ["mini pildoras"]
---

Las API permiten que programas informáticos se comuniquen entre sí.

Normalmente usarás una API (Application Programming Interface) sobre el protocolo HTTP, que es el más común en las comunicaciones en internet. Sin embargo, una API es cualquier conjunto de reglas que siguen dos sistemas informáticos para comunicarse. Normalmente no eres consciente de que estás consumiendo una API porque utilizas una librería que te lo facilita.

## ¿Qué tipos de API existen?

Si tienes alguna experiencia programando, seguro que estos ejemplos te van a sonar:

- **Sistemas operativos**: Windows, macOS y Linux proporcionan API a los desarrolladores de software para interactuar con las funciones y características del sistema operativo.
- **Bases de datos**: Permiten que tu software se conecte y realice consultas a los motores de bases de datos (MySQL, PosgreSQL, etc.).
- **Hardware**: Los dispositivos como impresoras, cámaras y sensores, a menudo vienen con sus propias API que permiten a los desarrolladores interactuar con ellos desde sus aplicaciones.
- **Servicios en la nube**: Ya sea una API de estadísticas de Pokemon, como tu sistema de almacenamiento en la nube, como un VPS que contrates, ofrecen API que te permiten automatizar acciones mediante peticiones (probablemente en HTTP, aunque existen más protocolos de comunicación).

## Ejemplos con Go

No me voy a extender con varios lenguajes, como en otros artículos, porque vas a entender qué es cada una de las API anteriormente mencionadas fácilmente.

Para crear o leer archivos, necesitarás interactuar con el sistema operativo.

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    file, err := os.Create("file.txt")
    if err != nil {
        fmt.Println("Error creating file:", err)
        return
    }
    defer file.Close()

    data := []byte("Hello world!")
    _, err = file.Write(data)
    if err != nil {
        fmt.Println("Error writing file:", err)
        return
    }
}
```

Cómo consumir una API HTTP con la librería estándar de Go.

```go
package main

import (
    "fmt"
    "net/http"
    "io/ioutil"
)

func main() {
    url := "https://jsonplaceholder.typicode.com/posts/1"
    response, err := http.Get(url)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer response.Body.Close()

    body, err := ioutil.ReadAll(response.Body)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }

    fmt.Println(string(body))
}
```

Para interactuar con una base de datos, en este caso sqlite.

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/mattn/go-sqlite3"
)

func main() {
    db, err := sql.Open("sqlite3", "test.db")
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer db.Close()

    rows, err := db.Query("SELECT name FROM users WHERE id = ?", 1)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer rows.Close()

    var name string
    if rows.Next() {
        err := rows.Scan(&name)
        if err != nil {
            fmt.Println("Error:", err)
            return
        }
        fmt.Println("Name:", name)
    } else {
        fmt.Println("User not found")
    }
}
```

Para interactuar con Hardware, puedes utilizar la famosa librería [gocv](https://github.com/hybridgroup/gocv).


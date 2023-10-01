---
title: "What is an API?"
description: ""
summary: ""
date: 2023-09-20T19:19:25+02:00
draft: false
tags: ["tiny pills"]
---

APIs allow computer programs to communicate with each other.

Normally you will use an API (Application Programming Interface) over the HTTP protocol, which is the most common in Internet communications. However, an API is any set of rules that two computer systems follow to communicate. Normally you are not aware that you are consuming an API because you use a library that makes it easier for you.

## What types of APIs exist?

If you have any programming experience, these examples will surely sound familiar to you:

- **Operating Systems**: Windows, macOS and Linux provide APIs to software developers to interact with the functions and features of the operating system.
- **Databases**: They allow your software to connect and make queries to database engines (MySQL, PosgreSQL, etc.).
- **Hardware**: Devices such as printers, cameras and sensors often come with their own APIs that allow developers to interact with them from their applications.
- **Cloud services**: Whether it is a Pokemon statistics API, such as your cloud storage system, or a VPS that you hire, they offer APIs that allow you to automate actions through requests (probably in HTTP, although there are more communication protocols).

## Examples with Go

I am not going to go into several languages, as in other articles, because you will easily understand what each of the APIs mentioned above is.

To create or read files, you will need to interact with the operating system.

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

How to consume an HTTP API with the Go standard library.

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

To interact with a database, in this case sqlite.

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

To interact with Hardware, you can use the famous library [gocv](https://github.com/hybridgroup/gocv).

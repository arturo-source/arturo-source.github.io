---
title: "¿Qué es MVC?"
slug: "que-es-mvc"
description: ""
summary: ""
date: 2023-10-01T17:42:58+02:00
draft: false
---

MVC es un patrón de diseño que separa una aplicación en tres componentes clave: Modelo (datos y lógica), Vista (interfaz de usuario) y Controlador (gestión de interacciones y flujo).

MVC (Modelo-vista-controlador o model-view-controller), se utiliza en el desarrollo de software, especialmente en el desarrollo de aplicaciones web. Este patrón nos facilita tener un código más mantenible, escalable, y entendible.

## ¿Por qué elegir MVC?

Podrás decir "yo llevo X tiempo programando y no he necesitado saber qué es MVC", pero la realidad es que utilizar patrones en programación te va a ayudar muchísimo. La programación existe desde antes de que tú nacieras, y ya ha habido otras personas que se han encontrado **con los mismos problemas que tú**. Esta última frase la puedes extender a tu lenguaje favorito, a tu motor de videojuegos, o a cualquier ámbito de la programación.

Y, volviendo al tema del post, ¿por qué MVC? Lo cierto es que existen nuevas arquitecturas de diseño de aplicaciones, más nuevas y quizá mejores, pero muchas de ellas se basan en MVC, por lo que aprender MVC te va a servir como una buena base para aprender el resto.

La intención de MVC es separar el código en los tres componentes ya mencionados (Modelo, Vista y Controlador), hasta tal punto que tres personas diferentes podrían trabajar en los tres campos distintos, sin necesidad de saber cómo está programado el otro campo. Lo único que tienen que conocer el uno del otro es cómo se comunican entre sí (al final habrá un ejemplo práctico, para que quede 100% claro).

De hecho, tal es la separación entre los tres componentes de MVC, que se podría cambiar uno de ellos, y la aplicación debería seguir funcionando. El mejor ejemplo es cuando tenemos un **Modelo** que se comunica con una base de datos MySQL, y cambiamos el **Modelo** completamente para que utilice PostgreSQL. La Vista y el Controlador pueden seguir siendo las mismas que antes.

## ¿Cómo se estructura MVC?

Como ya conoces las tres partes, y por qué es importante separarlas, lo siguiente es saber qué hace exactamente cada componente.

- **Vista**: Se trata de lo que verá el usuario de la aplicación. Puede ser HTML simple, una aplicación de React, o incluso una aplicación de móvil.
- **Controlador**: Actúa como intermediario entre la **Vista** y en **Modelo**. Procesa las solicitudes del usuario, y decide qué datos se muestran en la vista.
- **Modelo**: Es el encargado de tratar los datos. Cuando almacenas, actualizas, borras, o recuperas los datos, el **Controlador** no tiene que saber cómo se hace.

## Ejemplo práctico

El árbol de archivos que voy a usar tiene la siguiente forma:

```sh
view/
view/books.html
model/
model/book.go
controller/
controller/book.go
main.go
```

Usaré una API REST con Go como ejemplo, verás qué sencillo es de entender así. Tendremos un archivo HTML simple, que va a tener la sintaxis de un template de Go.

La **Vista** es este HTML que usando `range` recorrerá todos los libros. Archivo `./view/books.html`

```html
<html>
<head>
    <title>Lista de Libros</title>
</head>
<body>
    <h1>Lista de Libros</h1>
    <ul>
        {{range .}}
        <li>{{.Title}} - {{.Author}}</li>
        {{end}}
    </ul>
</body>
</html>
```

El **Controlador**, implemento solamente la lógica de obtener libros y añadir un libro, por simplicidad. Archivo `./controller/book.go`

```go
package controller

func bookListHandler(w http.ResponseWriter, r *http.Request) {
 if r.Method == http.MethodPost {
  newBook := model.Book{
   Title:  r.FormValue("title"),
   Author: r.FormValue("author"),
  }

  // Connect the controller with the model
  model.AddBook(newBook)
  fmt.Fprint(w, newBook)
 }

 if r.Method == http.MethodGet {
  // Connect the controller with the view 
  view, _ := template.ParseFiles("./view/books.html")

  // Connect the controller with the model
  books := model.GetBooks()   
  view.Execute(w, books)
 }
}
```

El **Modelo**, reemplazo la base de datos por un array como variable global, por simplicidad. Archivo `./model/book.go`

```go
package model

type Book struct {
 Title  string
 Author string
}

var books = []model.Book{
 {"Book 1", "Author 1"},
 {"Book 2", "Author 2"},
 {"Book 3", "Author 3"},
}

func GetBooks() []Book {
 // This could be a SELECT in a database
 // But return global books variable (used as database in this example) 
 return books
}

func AddBook(book Book) {
 // This could be an INSERT in a database
 // But append the book at global books variable (used as database in this example) 
 books = append(books, book)
}
```

Y, por último, la función principal para levantar el servidor HTTP. Archivo `./main.go`

```go
package main

func main() {
    http.HandleFunc("/books", controller.bookListHandler)

    http.ListenAndServe(":8080", nil)
}
```

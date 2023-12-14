---
title: "What is MVC?"
description: "A little bit of MVC to become a programming pro."
summary: "A little bit of MVC to become a programming pro."
date: 2023-10-01T17:42:52+02:00
draft: false
tags: ["tiny pills"]
---


MVC is a design pattern that separates an application into three key components: Model (data and logic), View (user interface), and Controller (interaction management and flow).

MVC (model-view-controller), is used in software development, especially in the development of web applications. This pattern makes it easier for us to have more maintainable, scalable, and understandable code.

## Why choose MVC?

You may say "I've been programming for X amount of time and I haven't needed to know what MVC is", but the reality is that using patterns in programming is going to help you a lot. Programming has been around since before you were born, and there have already been other people who have encountered **the same problems as you**. You can extend this last phrase to your favorite language, your video game engine, or any area of programming.

And, returning to the topic of the post, why MVC? The truth is that there are new application design architectures, newer and perhaps better, but many of them are based on MVC, so learning MVC will serve as a good foundation for learning the rest.

The intention of MVC is to separate the code into the three components already mentioned (Model, View and Controller), to such an extent that three different people could work on the three different fields, without needing to know how the other field is programmed. The only thing they have to know about each other is how they communicate with each other (at the end there will be a practical example, so that it is 100% clear).

In fact, such is the separation between the three MVC components, that you could change one of them, and the application should still work. The best example is when we have a **Model** that talks to a MySQL database, and we change the **Model** completely to use PostgreSQL. The View and Controller can remain the same as before.

## How is MVC structured?

Since you already know the three parts, and why it is important to separate them, the next thing is to know what exactly each component does.

- **View**: This is what the user of the application will see. It can be simple HTML, a React app, or even a mobile app.
- **Controller**: Acts as an intermediary between the **View** and the **Model**. Processes user requests, and decides what data is displayed in the view.
- **Model**: He is in charge of processing the data. When you store, update, delete, or retrieve data, the **Controller** does not have to know how it is done.

## Practical example

The file tree that I am going to use has the following form:

```sh
view/
view/books.html
model/
model/book.go
controller/
controller/book.go
main.go
```

I will use a REST API with Go as an example, you will see how easy it is to understand like this. We will have a simple HTML file, which will have the syntax of a Go template.

The **View** is this HTML that using `range` will cycle through all the books. File `./view/books.html`

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

The **Controller** implements only the logic of getting books and adding a book, for simplicity. File `./controller/book.go`

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

The **Model** replaced the database with an array as a global variable, for simplicity. File `./model/book.go`

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

And finally, the main function to set up the HTTP server. File `./main.go`

```go
package main

func main() {
    http.HandleFunc("/books", controller.bookListHandler)

    http.ListenAndServe(":8080", nil)
}
```

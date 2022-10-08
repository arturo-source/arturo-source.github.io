---
title: "Learn Golang from base (parte 2)"
description: "Learn more advanced concepts like reading a file, recursion, creating a server, and something else."
summary: "Learn more advanced concepts like reading a file, recursion, creating a server, and something else."
date: 2022-10-08T19:52:10+02:00
draft: false
---

Despite the fact that [in the previous post](/posts/learn-golang/) all the necessary bases to start learning programming were explained, what I am going to show now are somewhat more advanced concepts , which no longer make up something basic and necessary to start programming. I want them to serve as an example of how to continue learning, and finally, I will leave some resources so that you can continue learning on your own. After that, all you have to do is figure out what you want to program next.

## Recursion in Go

Recursion is something that exists in many languages, and is used in some computer problems because some problems turn out to be easier to solve. However, it is something that people often have a harder time understanding because it is not so easy to follow the flow of the program.

Next we will see some recursion problems, although not in all of them the best solution is the recursive one (sometimes the best solution is the common programming, which is called imperative).

### Solve the Fibonacci sequence with recursion

```go
func fibonacci(n int) int {
 if n <= 1 {
  return n
 }

 return fibonacci(n-1) + fibonacci(n-2)
}
```

As we have said before, the recursive solution is not usually the most efficient, as it is in this case. However, it is an elegant solution, and easy to read.

1. If the number is less than or equal to 1, we return the number (the fibonacci of 0 is 0, and the fibonacci of 1 is 1).
2. If not, we return the sum of the previous and the previous to the previous (the fibonacci of 2 is fibonacci(1) **1** + fibonacci(0) **0**, the one of 3 is fibonacci(2) **1** + fibonacci(1) **1**, and so on).

With these two simple rules, which are the same as those stated in the Fibonacci sequence, we can solve the problem recursively.

## Read a file with Go

Each programming language has its way of reading files, but each of them has its peculiarities. In Go we can go to the resource that I left you in the previous post (gobyexample). Personally, whenever I don't remember how to do it (it's usually common), I go to <https://gobyexample.com/reading-files>, and there I find a simple example.

Of course, it depends on what we want to do, if it is a very large file, if we want to read it by lines, if the file is an image, etc. And for this we may have to do some more searching. But the simplest way to do it is as follows:

```go
package main

import (
 "fmt"
 "os"
)

func main() {
 dat, err := os.ReadFile("/tmp/dat")
 if err != nil {
  panic(err)
 }
 fmt.Print(string(dat))
}
```

The first thing we do is import the `os` package, which is the one that allows us to read files. Then, in the `main` function, we call os.ReadFile, which receives as a parameter the path of the file we want to read. If everything went well, it returns an array of bytes, and an error. If the error is something other than `nil`, then there has been a problem, so we can stop the program with `panic`. It is not a good practice to use `panic` in normal program flow, but in this case, it is the easiest thing to do.

In case there is no error, we use fmt.Println that we have already seen in the previous post, to print the content of the file. But since the content of the file is a byte array, we convert it to a string with `string(dat)`.

### Read json file with Go

But, what is the point of reading a file to display its content? Well, not much, because we can do that with any program. What we will usually want to do is use the information that exists inside. In this case, we are going to read a json file, and we are going to use the information it contains.

```go
package main

import (
 "encoding/json"
 "fmt"
 "os"
)

type User struct {
 Username string `json:"username"`
 Age      int    `json:"age"`
}

// Read json file
func ReadJson(filepath string) (User, error) {
 var user User
 file, err := os.Open(filepath)
 if err != nil {
  return user, err
 }
 defer file.Close()

 err = json.NewDecoder(file).Decode(&user)
 return user, err
}

func main() {
 user, err := ReadJson("user.json")
 if err != nil {
  panic(err)
 }

 fmt.Println("Username:", user.Username)
 fmt.Println("Age:", user.Age)
}
```

This code is bigger than what we've seen so far, but we're going to go through it piece by piece so you can see how it works. We will first import `encoding/json`, which is the package that allows us to read json files. Along with `os` and `fmt`, which we have already seen.

If you have come this far, I assume you know what JSON is, but just in case, I leave you this Wikipedia link that may help you: <https://en.wikipedia.org/wiki/JSON>.

In other languages ​​like JavaScript, there is no need to define the structure, since the json can be read without knowledge of what the object is like. But in Go, we can't do that, so we have to define the structure (just like in TypeScript).

Next, we define a `User` structure, which is what we are going to find when reading the json. In this case, the json has two fields, `username` and `age`, and we are going to store them in the `User` structure. To do this, we use the `json` tag that allows us to indicate the name of the field in the json.

**Now let's get to the important stuff**. The `ReadJson` function receives as a parameter the path of the file we want to read. In this case, the file is called `user.json`. The function returns a `User`, and an error. If all went well, the error will be `nil`, as we have seen before. This is common behavior in Go, letting us know if there have been any errors.

At the beginning of the function we define a `user` variable of type `User`, which is what we are going to return. Then we open the file with `os.Open`, and if there were any errors, we return it. If not, we close the file with `defer file.Close()`. This is important, because if we don't close the file, it can cause problems. `defer` allows us to execute a function at the end of the function in which it is defined. In this case, `file.Close()` will be executed at the end of the `ReadJson` function.

Next, we create a JSON decoder, which will convert the bytes of the file (the text that we can read when opening it with a text editor), into a Go object. To do this, we use `json.NewDecoder(file)`. And we store it in the `user` variable. To do this, we use `Decode(&user)`. The `&` is something we haven't seen so far, but I'm leaving it as homework for you to investigate on your own.

And finally, we return the user and the error. If all went well, the error will be `nil`.

## Web server with Go

And this is the last thing we are going to see in this post. The rest of the learning you will have to do by yourselves, and for this I leave you some links at the end that can help you.

Let's create a web server in Go. To do this, we are going to use the `net/http` package, which is what allows us to create web servers. In other languages you would have to choose an external library, but in Go, it is already included. Then you can research frameworks that make your life easier, like `gin` or `echo`. However, Go's default library is quite complete.

```go
package main

import (
 "fmt"
 "net/http"
)

func hello(w http.ResponseWriter, req *http.Request) {
 fmt.Fprintf(w, "hello\n")
}

func headers(w http.ResponseWriter, req *http.Request) {
 for name, headers := range req.Header {
  for _, h := range headers {
   fmt.Fprintf(w, "%v: %v\n", name, h)
  }
 }
}

func main() {

 http.HandleFunc("/hello", hello)
 http.HandleFunc("/headers", headers)

 http.ListenAndServe(":8090", nil)
}
```

This example is again from gobyexample <https://gobyexample.com/http-server>. As we did before, the first thing will be to import the necessary libraries. For this example we will use the aforementioned `net/http`.

In this case, we have two functions, `hello` and `headers`. The first one simply returns a text, and the second one returns the headers of the request. For these concepts we may need some knowledge about http, but since it is not the subject to be treated, simply enter the url <https://localhost:8090/hello> and <https://localhost:8090/headers> to see the result.

The only thing we will do is define two routes, `/hello` and `/headers`, and associate them with the functions that we have defined before. Note that the functions no longer carry () as in all the occasions that we have used them before. This is because we are not executing them, but passing them as a parameter to another function (as if it were an `int`, or a `float`).

Lastly, we start the web server on port 8090.

### Rest API with Go

Effectively, we already have a web server, with just a few lines of code. But what if we want the server to return a json? Or receive a json? Or that it returns a json based on the parameters that we pass to it? Well, for that we may need a framework, which can make our lives easier, such as the aforementioned `gin` or `echo`.

But let's use what we learned throughout the post, and we can create a web server that works with json with the knowledge we have.

```go
package main

import (
 "encoding/json"
 "net/http"
)

func myFunc(w http.ResponseWriter, req *http.Request) {
 json.NewEncoder(w).Encode(map[string]string{"hello": "world"})
}

func main() {
 http.HandleFunc("/myJson", myFunc)
 http.ListenAndServe(":8090", nil)
}
```

With what you have learned, it should be easy to understand what is in this example. But let's go with the explanation line by line.

First, we import the `encoding/json` and `net/http` libraries. Next, we create a function `myFunc`. And in the main, as already mentioned, the first line declares the path `/myJson` that the `myFunc` function will respond to, and the second line starts the web server on port 8090.

What happens in the `myFunc` function should be familiar to us, because before we have done it to decode (by reading the json file), but now what we want to do is encode it, this way we can send the information as bytes (the text that we can read with a text editor) to the client.

The weirdest thing is `map[string]string{"hello": "world"}`, but it's just a Go dictionary. In this case, the key is a string, and so is the value. You can research them at <https://gobyexample.com/maps>.

## Conclusion

Now there will be more questions in the air, but this is all part of learning. What you will have to do is investigate on your own, because that is what applies in real life when it comes to programming. When you have an idea about the project you want to do, search the internet for how to do it, surely there is someone who has had the same problem as you before.

If you want to continue learning in Go, I recommend you the following resources that will be a great help:

* <https://go.dev/tour/> is an interactive Go tutorial, which will help you understand the concepts from the very basics to more advanced topics.
* <https://roadmap.sh/golang> is used to learn about the most popular Go libraries and how they relate to your needs depending on the project.

---
title: "Execute Golang in the Browser"
description: "Mini tutorial to execute Go with WASM in the browser"
summary: "Mini tutorial to execute Go with WASM in the browser"
date: 2024-01-25T19:36:46+01:00
draft: false
---

The goal of this article is to convert my terminal application [poker-odds](https://github.com/arturo-source/poker-odds), into an application that can be used in the browser.

For this example I will use `tinygo`, which is a Go compiler for microprocessors, which also produces WASM code. If you want to see how it's done with the regular Go compiler, you can see it [on the Go wiki](https://github.com/golang/go/wiki/WebAssembly).

## Compile Go code

Remember to have the [`tinygo` compiler installed](https://tinygo.org/getting-started/install/). To compile the Go code to WASM code, I will use the following command:

```sh
tinygo build -o wasm.wasm -target wasm ./main.go
```

- **tinygo** is the compiler.
- **build** is used to compile the code.
- **-o wasm.wasm** I tell you where I want the compiled WASM to be saved.
- **-target wasm** I indicate that I want to compile to WASM, and not to a microprocessor.
- **./main.go** is where the Go code is.

Inside the **./main.go** file, I will have code like the following:

```go
package main

func main() {}

//export multiply
func multiply(x, y int) int {
    return x * y;
}
```

The `//export multiply` comment is **very important**. Without it, you won't be able to run the function from JavaScript.

## Run Go code from JavaScript

Once the code has been compiled, you have to load the code in the browser, the easiest way is to create an `index.html`.

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <script src="/wasm_exec.js"></script>
    <script>
        const go = new Go(); // Defined in wasm_exec.js
        const WASM_URL = '/wasm.wasm';

        var wasm;

        if ('instantiateStreaming' in WebAssembly) {
            WebAssembly.instantiateStreaming(fetch(WASM_URL), go.importObject).then(function (obj) {
                wasm = obj.instance;
                go.run(wasm);
            })
        } else {
            fetch(WASM_URL).then(resp =>
                resp.arrayBuffer()
            ).then(bytes =>
                WebAssembly.instantiate(bytes, go.importObject).then(function (obj) {
                    wasm = obj.instance;
                    go.run(wasm);
                })
            )
        }
    </script>
</head>
<body>
</body>
</html>
```

Two scripts remain visible. The first loads a `wasm_exec.js` file. It's important, because it's what allows you to execute `const go = new Go();` later. You can find this file within your operating system by running the following command in Linux:

```sh
find / -name wasm_exec.js
```

This command will search for all the files on your operating system called `wasm_exec.js`. I will copy it to the same folder as my `index.html`.

You'll also find it at <https://github.com/tinygo-org/tinygo/blob/release/targets/wasm_exec.js>, but you may have problems if the GitHub version of `tinygo` is not the same as yours. .

### Start an HTTP server

There are multiple ways to have an HTTP server quickly. Open a terminal, and go to the folder where you have `wasm_exec.js`, `index.html`, and `wasm.wasm`.

With interpreted languages like python you can simply put `python -m http.server 8000`. For PHP `php -S 127.0.0.1:8000`. Either command will open an HTTP server on port 8000, but if you prefer to build a small server with Go, copy the following:

```go
package main

import (
 "flag"
 "log"
 "net/http"
)

func main() {
 port := flag.String("p", "8000", "port to serve")
 directory := flag.String("d", ".", "directory to host")
 flag.Parse()

 http.Handle("/", http.FileServer(http.Dir(*directory)))

 log.Printf("Serving %s on: http://127.0.0.1:%s\n", *directory, *port)
 log.Fatal(http.ListenAndServe(":"+*port, nil))
}
```

Compile the code with `go build`, and run it. Remember that you can change the `-p` options to change the port, and `-d` for the folder you want to serve.

### Now, let's multiply using Go from JavaScript

After a long process, I can now run the WASM code. I enter the url <http://127.0.0.1:8000/>, a blank page appears, but if I open the developer tools (by pressing `F12`, `ctrl + shift + i`, or whatever is done in your browser/operating system). And in the JavaScript terminal I type `wasm.exports.multiply(4,5)`, and the function returns 20.

## Create the poker-odds tool in HTML

Now I have a function that multiplies in Go, which can be called from JavaScript, using WASM. It doesn't have to be that difficult to port my code. The first thing I'm going to do is fork my repository, because the application is completely designed to be used from the terminal, so quite a few things will have to change. [This is the new repository that will work with WASM](https://github.com/arturo-source/poker-odds-wasm).

### Fork poker-odds

To do the fork, I had to create a repository from scratch (because you can't fork your own repositories), clone the original repository, change the remote origin url, and push to the new repository. With the following commands:

```sh
git clone git@github.com:arturo-source/poker-odds.git poker-odds-wasm
cd poker-odds-wasm/
git remote -v # show old repositories
git remote set-url origin git@github.com:arturo-source/poker-odds-wasm.git
git push -u origin main
```

### Modify the program to work with WASM

The strategy I am going to follow is that the Go program itself returns the HTML. Another option would be to run the logic in Go, return the result to JavaScript, and manipulate the DOM to insert the data.

The latter is usually recommended because manipulating the DOM with WASM is more expensive than with JavaScript, but I have found incompatibilities between Go and WASM that do not allow me to port some functions made in Go, so that they are executed in JS (for example, in Go a function can return multiple values, and when you compile it to WASM it doesn't work).

An advantage of creating the HTML from Go is that the state of the application is maintained in Go, in JavaScript I will only make a function to embed the HTML. Managing state from Go makes the program logic simpler.

The modifications will be the following:

- Delete the file `console.go` (contains how to print or read the console).
- Extract the `func main` into a `func getResultsInHTML` to be able to export it, and be able to use it through WASM.
- Use `func parseCommandLine` logic to parse user input with `func parseUserInputs`.
- Use an HTML template that emits a result similar to `func printResults`.

Once I have programmed these changes, it is time to compile, I run `tinygo build -o wasm.wasm -target wasm ./main.go` again, I insert the `wasm.wasm` into the HTML as I did with the example to multiply, and...

## WTF!? It does not work

After all this time adapting the program, I get the error `panic: unimplemented: (reflect.Type).NumOut()`. I already suspected that the `tinygo` compiler would not be as complete as the `go` one, but I can still try the official compiler.

For your information, this is the error I'm getting:

```txt
Uncaught RuntimeError: unreachable
    at runtime._panic (wasm.wasm:0x2b2a)
    at (poker-odds-wasm/*reflect.rawType).NumOut (http://localhost:8000/poker-odds-wasm/wasm.wasm)
    at interface:{Align:func:{}{basic:int},AssignableTo:func:{named:reflect.Type}{basic:bool},Bits:func:{}{basic:int},ChanDir:func:{}{named:reflect.ChanDir},Comparable:func:{}{basic:bool},ConvertibleTo:func:{named:reflect.Type}{basic:bool},Elem:func:{}{named:reflect.Type},Field:func:{basic:int}{named:reflect.StructField},FieldAlign:func:{}{basic:int},FieldByIndex:func:{slice:basic:int}{named:reflect.StructField},FieldByName:func:{basic:string}{named:reflect.StructField,basic:bool},FieldByNameFunc:func:{func:{basic:string}{basic:bool}}{named:reflect.StructField,basic:bool},Implements:func:{named:reflect.Type}{basic:bool},In:func:{basic:int}{named:reflect.Type},IsVariadic:func:{}{basic:bool},Key:func:{}{named:reflect.Type},Kind:func:{}{named:reflect.Kind},Len:func:{}{basic:int},Method:func:{basic:int}{named:reflect.Method},MethodByName:func:{basic:string}{named:reflect.Method,basic:bool},Name:func:{}{basic:string},NumField:func:{}{basic:int},NumIn:func:{}{basic:int},NumMethod:func:{}{basic:int},NumOut:func:{}{basic:int},Out:func:{basic:int}{named:reflect.Type},PkgPath:func:{}{basic:string},Size:func:{}{basic:uintptr},String:func:{}{basic:string}}.NumOut$invoke (wasm.wasm:0x935ed)
    at text/template.goodFunc (wasm.wasm:0x9358e)
    at text/template.addValueFuncs (wasm.wasm:0x944da)
    at (poker-odds-wasm/*text/template.Template).Funcs (http://localhost:8000/poker-odds-wasm/wasm.wasm)
    at (poker-odds-wasm/*html/template.Template).Execute (http://localhost:8000/poker-odds-wasm/wasm.wasm)
    at getResultsInHTML (wasm.wasm:0xb6ce2)
    at getResultsInHTML.command_export (wasm.wasm:0xba714)
    at HTMLFormElement.<anonymous> (poker-odds-wasm/:119:35)
```

### Differences between the official go compiler and tinygo

The reason for using `tinygo` was that it is designed for lower memory architectures, and produces smaller executables, but I have found that it is also significantly slower. Specifically, on my computer with Linux/amd64 architecture, I have obtained the following results:

||tinygo|go build|go build -ldflags="-s -w"|
|-|-----|--------|-------------------------|
|version|0.30.0|1.21.6|1.21.6|
|time|14,536s|0,080s|0,086s|
|size|1,8M|2,6M|2,5M|
|size syscall/js|?|5,2M|5,1M|

### Reinserting the WASM into the browser

There seems to be another problem, `Uncaught TypeError: Cannot read properties of undefined (reading 'exports')`. Nothing strange, I just have to use the `wasm_exec.js` of my version of Go, which is different from the version of tinygo. I search for it with the `find` command, as before, and copy it from the path `/usr/lib/go/misc/wasm/wasm_exec.js`.

A new error appears `Uncaught TypeError: wasm.exports.getResultsInHTML is not a function`, it must be because the Go compiler exports the functions in a different way, not with `//export getResultsInHTML`.

To communicate with JS I will need the package <https://pkg.go.dev/syscall/js>. Functions are registered from `main`, and the program cannot terminate, so the thread is blocked using a `<-make(chan bool)` channel. Furthermore, the function must be of the type `js.FuncOf` -> `func(this js.Value, args []js.Value) any`.

```go
func main() {
 js.Global().Set("getResultsInHTML", js.FuncOf(func(this js.Value, args []js.Value) any {
  handsStr := args[0].String()
  boardStr := args[1].String()
  return getResultsInHTML(handsStr, boardStr)
 }))

 // listen infinite
 <-make(chan bool)
}
```

Now the function is global in JavaScript, it is not called with `wasm.exports.getResultsInHTML` but directly `getResultsInHTML`.

## Use poker-odds

Below I have inserted an iframe with the necessary code, so you can test the final result.

{{< iframe src="/poker-odds-wasm/">}}

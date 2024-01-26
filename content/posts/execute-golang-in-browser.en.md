---
title: "Execute Golang in the Browser"
description: "Mini tutorial to execute Go with WASM in the browser"
summary: "Mini tutorial to execute Go with WASM in the browser"
date: 2024-01-25T19:36:46+01:00
draft: true
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

Now I have a function that multiplies in Go, which can be called from JavaScript, using WASM. It doesn't have to be that difficult to port my code. The first thing I'm going to do is fork my repository, because the application is completely designed to be used from the terminal, so quite a few things will have to change. [This is the new repository that will work with WASM]().

...

## Use poker-odds

Below I have inserted an iframe with the necessary code, so you can test the final result.

{{< iframe src="/poker-odds-wasm/">}}

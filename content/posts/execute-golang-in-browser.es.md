---
title: "Execute Golang in Browser"
slug: "ejecutar-golang-en-el-navegador"
description: "Mini tutorial para ejecutar Golang en el navegador, usando WASM."
summary: "Mini tutorial para ejecutar Golang en el navegador, usando WASM."
date: 2024-01-25T19:36:48+01:00
draft: true
---

El objetivo de este artículo es convertir mi aplicación de terminal [poker-odds](https://github.com/arturo-source/poker-odds), en una aplicación que se pueda usar en el naveegador.

Para este ejemplo utilizaré `tinygo`, que es un compilador de Go para microprocesadores, que también produce código WASM. Si quieres ver cómo se hace con el compilador de Go normal, puedes verlo [en la wiki de Go](https://github.com/golang/go/wiki/WebAssembly).

## Compilar el código de Go

Recuerda tener el [compilador de `tinygo` instalado](https://tinygo.org/getting-started/install/). Para compilar el código de Go a código WASM, utilizaré el siguiente comando:

```sh
tinygo build -o wasm.wasm -target wasm ./main.go
```

- **tinygo** es el compilador.
- **build** sirve para compilar el código.
- **-o wasm.wasm** le indico dónde quiero que se guarde el WASM compilado.
- **-target wasm** le indico que quiero compilar a WASM, y no a un microprocesador.
- **./main.go** es donde está el código Go.

Dentro del archivo **./main.go**, tendré un código como el siguiente:

```go
package main

func main() {}

//export multiply
func multiply(x, y int) int {
    return x * y;
}
```

Es **muy importante** el comentario `//export multiply`. Sin él, no podrás ejecutar la función desde JavaScript.

## Ejecutar el código de Go desde JavaScript

Una vez el código ha sido compilado, hay que cargar el código en el navegador, la manera más sencilla es creando un `index.html`.

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

A la vista quedan dos scripts. El primero carga un archivo `wasm_exec.js`. Es importante, porque es lo que hace que puedas ejecutar `const go = new Go();` posteriormente. Puedes encontrar este fichero dentro de tu sistema operativo, ejecutando el siguiente comando en Linux:

```sh
find / -name wasm_exec.js
```

Este comando buscará todos los archivos en tu sistema operativo llamados `wasm_exec.js`. Yo lo copiaré en la misma carpeta que mi `index.html`.

También lo encontrarás en <https://github.com/tinygo-org/tinygo/blob/release/targets/wasm_exec.js>, pero puedes tener problemas si la versión de `tinygo` de GitHub no es la misma que la tuya.

### Iniciar un servidor HTTP

Existen multiples formas de tener un servidor HTTP rápidamente. Abre una terminal, y ve a la carpeta donde tengas el `wasm_exec.js`, `index.html`, y `wasm.wasm`.

Con lenguajes interpretados como python puedes poner simplemente `python -m http.server 8000`. Para PHP `php -S 127.0.0.1:8000`. Cualquiera de los dos comandos abrirá un servidor HTTP en el puerto 8000, pero si prefieres compilar un pequeño servidor con Go, copia lo siguiente:

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

Compila el código con `go build`, y ejecutalo. Recuerda que puedes cambiar las opciones `-p` para cambiar el puerto, y `-d` para la carpeta que quieres servir.

### Ahora sí, a multiplicar usando Go desde JavaScript

Después de un largo proceso, ya puedo ejecutar el código WASM. Entro a la url <http://127.0.0.1:8000/>, me aparece una página en blanco, pero si abro las herramientas de desarrollador (pulsando `F12`,  `ctrl + shift + i`, o como se haga en tu navegador/sistema operativo). Y en la terminal de JavaScript escribo `wasm.exports.multiply(4,5)`, y la función devuelve 20.

## Crear la herramienta de poker-odds en HTML

Ahora ya tengo una función que multiplica en Go, que se puede llamar desde JavaScript, mediante WASM. No tiene que ser tan difícil portar mi código. Lo primero que voy a hacer es un fork de mi repositorio, porque la aplicación está completamente pensada para ser usada desde terminal, así que habrá que cambiar bastantes cosas. [Este es el nuevo repositorio que funcionará con WASM]().

...

## Usar poker-odds

A continuación he insertado un iframe con el código necesario, para que puedas probar el resultado final.

{{< iframe src="/poker-odds-wasm/">}}

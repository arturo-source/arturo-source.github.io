---
title: "Execute Golang in Browser"
slug: "ejecutar-golang-en-el-navegador"
description: "Mini tutorial para ejecutar Golang en el navegador, usando WASM."
summary: "Mini tutorial para ejecutar Golang en el navegador, usando WASM."
date: 2024-01-25T19:36:48+01:00
draft: false
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

Ahora ya tengo una función que multiplica en Go, que se puede llamar desde JavaScript, mediante WASM. No tiene que ser tan difícil portar mi código. Lo primero que voy a hacer es un fork de mi repositorio, porque la aplicación está completamente pensada para ser usada desde terminal, así que habrá que cambiar bastantes cosas. [Este es el nuevo repositorio que funcionará con WASM](https://github.com/arturo-source/poker-odds-wasm).

### Hacer el fork de poker-odds

Para hacer el fork, he tenido que crear un repositorio desde cero (porque no puedes hacer forks de tus propios repositorios), clonar el repositorio original, cambiar el remote origin url, y hacer un push al nuevo repositorio. Con los siguientes comandos:

```sh
git clone git@github.com:arturo-source/poker-odds.git poker-odds-wasm
cd poker-odds-wasm/
git remote -v # show old repositories
git remote set-url origin git@github.com:arturo-source/poker-odds-wasm.git
git push -u origin main
```

### Modificar el programa para que funcione con WASM

La estrategia que voy a seguir es, que el propio programa de Go devuelva el HTML. Otra opción sería ejecutar la lógica en Go, devolver el resultado a JavaScript, y manipular el DOM para insertar los datos.

Habitualmente se recomienda esta última porque manipular el DOM con WASM es más costoso que con JavaScript, pero he encontrado incompatibilidades entre Go y WASM que no me permiten portar algunas funciones hechas en Go, para que se ejecuten en JS (por ejemplo, en Go una función puede devolver multiples valores, y cuando lo compilas a WASM no funciona).

Una ventaja de crear el HTML desde Go es que el estado de la aplicación se mantiene en Go, en JavaScript sólo haré una función para incrustar el HTML. Manejar el estado desde Go hace más sencilla la lógica del programa.

Las modificaciones serán las siguientes:

- Eliminar el archivo `console.go` (contiene cómo imprimir o leer la consola).
- Extraer el `func main` en un `func getResultsInHTML` para poder exportarlo, y poder usarlo mediante WASM.
- Usar la lógica de `func parseCommandLine` para parsear el input del usuario con `func parseUserInputs`.
- Usar un template en HTML que emita un resultado parecido a `func printResults`.

Una vez he programado estos cambios, llega la hora de compilar, vuelvo a ejecutar `tinygo build -o wasm.wasm -target wasm ./main.go`, inserto el `wasm.wasm` en el HTML como he hecho con el ejemplo de multiplicar, y...

## WTF!? No funciona

Después de todo este tiempo adaptando el programa, obtengo el error `panic: unimplemented: (reflect.Type).NumOut()`. Ya sospechaba que el compilador de `tinygo` no sería tan completo como el de `go`, pero aún puedo probar con el compilador oficial.

Para los interesados, este es el error que estoy obteniendo:

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

### Diferencias entre el compilador oficial de go y tinygo

El motivo para utilizar `tinygo` era que este está preparado para arquitecturas con menos memoria, y produce ejecutables más pequeños, pero he descubierto que también es significativamente más lento. En concreto, en mi ordenador con arquitectura linux/amd64, he obtenido los siguientes resultados:

||tinygo|go build|go build -ldflags="-s -w"|
|-|-----|--------|-------------------------|
|version|0.30.0|1.21.6|1.21.6|
|time|14,536s|0,080s|0,086s|
|size|1,8M|2,6M|2,5M|
|size syscall/js|?|5,2M|5,1M|

### Vuelvo a insertar el WASM en el navegador

Parece que hay otro problema, `Uncaught TypeError: Cannot read properties of undefined (reading 'exports')`. Nada raro, solo tengo que utilizar el `wasm_exec.js` de mi versión de Go, que es distinto al de la versión de tinygo. Lo busco con el comando `find`, como antes, y lo copio de la ruta `/usr/lib/go/misc/wasm/wasm_exec.js`.

Un nuevo error aparece `Uncaught TypeError: wasm.exports.getResultsInHTML is not a function`, debe ser porque el compilador de Go exporta las funciones de una manera distinta, no con `//export getResultsInHTML`.

Para comunicar con JS necesitaré el paquete <https://pkg.go.dev/syscall/js>. Las funciones se registran desde el `main`, y el programa no puede terminar, así que se bloquea el hilo utilizando un canal `<-make(chan bool)`. Además la función tiene que ser del tipo `js.FuncOf` -> `func(this js.Value, args []js.Value) any`.

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

Ahora la función es global en JavaScript, no se llama con `wasm.exports.getResultsInHTML` sino directamente `getResultsInHTML`.

## Usar poker-odds

A continuación he insertado un iframe con el código necesario, para que puedas probar el resultado final.

{{< iframe src="/poker-odds-wasm/">}}

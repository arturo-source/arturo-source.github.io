---
title: "Aprende el lenguaje Go desde 0 (parte 2)"
slug: "aprende-go-desde-cero-parte-2"
description: "Aprende conceptos más avanzados como leer un archivo, recursión, crear un servidor, y más."
summary: "Aprende conceptos más avanzados como leer un archivo, recursión, crear un servidor, y más."
date: 2022-10-08T19:52:10+02:00
draft: false
---

A pesar de que [en el anterior post](/es/posts/aprende-go-desde-cero/) se explicasen todas las bases necesarias para comenzar a aprender en la programación, lo que voy a mostrar ahora son conceptos algo más avanzados, los cuales ya no conforman algo básico y necesario para comenzar a programar. Quiero que sirvan como ejemplo de cómo continuar aprendiendo, y por último, dejaré unos recursos para que podáis seguir aprendiendo por vuestra cuenta. Después de eso solo faltará que imaginéis qué es lo que queréis programar después.

## Recursión en Go

La recursión es algo que existe en muchos lenguajes, y que se utiliza en algunos problemas informáticos debido a que algunos problemas resultan ser más sencillos de resolver. Sin embargo, es algo que a la gente le suele costar más entender porque no es tan fácil seguir el flujo del programa.

A continuación veremos algunos problemas de recursión, aunque no en todos la mejor solución es la recursiva (a veces la mejor solución es la programación común, que se llama imperativa).

### Resolver la sucesión de Fibonacci con recursión

```go
func fibonacci(n int) int {
 if n <= 1 {
  return n
 }

 return fibonacci(n-1) + fibonacci(n-2)
}
```

Como bien hemos dicho antes, la solución recursiva no suele ser la más eficiente, como sucede en este caso. Sin embargo, resulta una solución elegante, y sencilla de leer.

1. Si el número es menor o igual a 1, devolvemos el número (el fibonacci de 0 es 0, y el de 1 es 1).
2. Si no, devolvemos la suma del anterior y el anterior al anterior (el fibonacci de 2 es fibonacci(1) **1** + fibonacci(0) **0**, el de 3 es fibonacci(2) **1** + fibonacci(1) **1**, y así sucesivamente).

Con estas dos simples reglas, que son las mismas que se enuncian en la sucesión de Fibonacci, podemos resolver el problema de forma recursiva.

## Leer archivos en Go

Cada lenguaje de programación tiene su forma de leer archivos, pero cada uno de ellos tiene sus peculiaridades. En Go podemos acudir al recurso que os dejé en el post anterior (gobyexample). Personalmente, siempre que no recuerdo cómo hacerlo (suele ser común), acudo a <https://gobyexample.com/reading-files>, y allí encuentro un simple ejemplo.

Por supuesto, depende de lo que queramos hacer, si es un archivo muy grande, si queremos leerlo por líneas, si el archivo es una imagen, etc. Y para ello puede que nos toque hacer alguna búsqueda más. Pero la manera más simple de hacerlo es la siguiente:

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

Lo primero que hacemos es importar el paquete `os`, que es el que nos permite leer archivos. Después, en la función `main` llamamos a os.ReadFile, que recibe como parámetro la ruta del archivo que queremos leer. Si todo ha ido bien, nos devuelve un array de bytes, y un error. Si el error es distinto de `nil`, es que ha habido algún problema, y por tanto, podemos parar el programa con `panic`. No es una práctica recomendable usar `panic` en el flujo de un programa normal, pero en este caso, es lo más sencillo.

En caso de que no haya error, utilizamos fmt.Println que ya hemos visto en el post anterior, para imprimir el contenido del archivo. Pero como el contenido del archivo es un array de bytes, lo convertimos a string con `string(dat)`.

### Leer json en Go

Pero, ¿de qué nos sirve leer un archivo para mostrar su contenido? Pues de poco, porque eso lo podremos hacer con cualquier programa. Lo que querremos hacer habitualmente es utilizar la información que existe dentro. En este caso, vamos a leer un archivo json, y vamos a utilizar la información que contiene.

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

Este código es más grande de lo que hemos visto hasta ahora, pero vamos a ir parte por parte para que veáis cómo funciona. Primero importaremos `encoding/json`, que es el paquete que nos permite leer archivos json. Junto con `os` y `fmt`, que ya hemos visto.

Si habéis llegado hasta aquí, doy por hecho que sabéis lo que es un JSON, pero por si acaso, os dejo este enlace de la Wikipedia que os puede servir de ayuda: <https://es.wikipedia.org/wiki/JSON>.

En otros lenguajes como JavaScript, no hace falta definir la estructura, ya que el json se puede leer sin conocimiento de cómo es el objeto. Pero en Go, no podemos hacerlo, y por tanto, tenemos que definir la estructura (al igual que sucede en TypeScript).

Después, definimos una estructura `User`, que es la que vamos a encontrarnos al leer el json. En este caso, el json tiene dos campos, `username` y `age`, y los vamos a guardar en la estructura `User`. Para ello, utilizamos la etiqueta `json` que nos permite indicar el nombre del campo en el json.

**Ahora vamos con lo importante**. La función `ReadJson` recibe como parámetro la ruta del archivo que queremos leer. En este caso, el archivo se llama `user.json`. La función devuelve un `User`, y un error. Si todo ha ido bien, el error será `nil`, tal cual hemos visto antes. Este es un comportamiento común en Go, que nos permite saber si ha habido algún error.

Al principio de la función definimos una variable `user` de tipo `User`, que es la que vamos a devolver. Después, abrimos el archivo con `os.Open`, y si ha habido algún error, lo devolvemos. Si no, cerramos el archivo con `defer file.Close()`. Esto es importante, porque si no cerramos el archivo, puede provocar problemas. `defer` nos permite ejecutar una función al final de la función en la que se ha definido. En este caso, se ejecutará `file.Close()` al final de la función `ReadJson`.

Después, creamos un decodificador de JSON, que va a convertir los bytes del archivo (el texto que podemos leer al abrirlo con un editor de texto), en un objeto de Go. Para ello, utilizamos `json.NewDecoder(file)`. Y lo guardamos en la variable `user`. Para ello, utilizamos `Decode(&user)`. El `&` es algo que no hemos visto hasta ahora, pero lo dejo como deberes para que investigueis por vuestra cuenta.

Y por último, devolvemos el usuario y el error. Si todo ha ido bien, el error será `nil`.

## Servidor web en Go

Y esto es lo último que vamos a ver en este post. El resto del aprendizaje tendréis que hacerlo por vosotros mismos, y para ello os dejo algunos enlaces al final que os pueden servir de ayuda.

Vamos a crear un servidor web en Go. Para ello, vamos a utilizar el paquete `net/http`, que es el que nos permite crear servidores web. En otros lenguajes tendrías que elegir una librería externa, pero en Go, ya viene incluida. Después puedes investigar sobre frameworks que te faciliten la vida, como `gin` o `echo`. Sin embargo, la librería por defecto de Go es bastante completa.

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

Este ejemplo es de nuevo de gobyexample <https://gobyexample.com/http-server>. Como anteriormente hicimos, lo primero será importar las librerías necesarias. Para este ejemplo usaremos la ya mencionada `net/http`.

En este caso, tenemos dos funciones, `hello` y `headers`. La primera simplemente devuelve un texto, y la segunda devuelve los headers de la petición. Para estos conceptos puede que necesitemos algo de conocimientos sobre http, pero como no es el tema a tratar, simplemente entrad a la url <https://localhost:8090/hello> y <https://localhost:8090/headers> para ver el resultado.

Lo único que haremos será definir dos rutas, `/hello` y `/headers`, y las asociamos a las funciones que hemos definido antes. Nota que las funciones ya no llevan () como en todas las ocasiones que las hemos usado antes. Esto es porque no las estamos ejecutando, sino que las estamos pasando como parámetro a otra función (como si fuera un `int`, o un `float`).

Por último, iniciamos el servidor web en el puerto 8090.

### Rest API con Go

Efectivamente, ya tenemos un servidor web, con tan solo unas pocas líneas de código. Pero, ¿qué pasa si queremos que el servidor devuelva un json? ¿O que reciba un json? ¿O que devuelva un json en función de los parámetros que le pasemos? Pues para eso puede que requiramos de un framework, que nos puede facilitar la vida, como los ya mencionados `gin` o `echo`.

Pero usemos lo aprendido a lo largo del post, y podemos crear un servidor web que trabaje con json con los conocimientos que tenemos.

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

Con lo ya aprendido, debería ser sencillo comprender lo que hay en este ejemplo. Pero vamos con la explicación línea a línea.

Primero, importamos las librerías `encoding/json` y `net/http`. Después, creamos una función `myFunc`. Y en el main, lo ya comentado, la primera línea declara la ruta `/myJson` que responderá la función `myFunc`, y la segunda inicia el servidor web en el puerto 8090.

Lo que sucede en la función `myFunc` nos debería sonar, porque antes lo hemos hecho para decodificar (al leer el archivo json), pero ahora lo que queremos hacer es codificarlo, de esta manera podremos mandar la información como bytes (el texto que podemos leer con un editor de texto) al cliente.

Lo más extraño es `map[string]string{"hello": "world"}`, pero es simplemente un diccionario de Go. En este caso, la clave es un string, y el valor también. Puedes investigar sobre ellos en <https://gobyexample.com/maps>.

## Conclusión

Ahora quedarán más preguntas en el aire, pero todo ello es parte del aprendizaje. Lo que tendréis que hacer es investigar por vuestra cuenta, porque es lo que se aplica en la vida real cuando se trata de la programación. Cuando tengáis una idea sobre el proyecto que queréis hacer, buscad en internet cómo se hace, que seguro que hay alguien que ha tenido el mismo problema que vosotros anteriormente.

Si queréis continuar con el aprendizaje en Go, os recomiendo los siguientes recursos que serán de gran ayuda:

* <https://go.dev/tour/> es un tutorial interactivo de Go, que os ayudará a entender los conceptos desde lo más básico hasta temas más avanzados.
* <https://roadmap.sh/golang> sirve para conocer las librerías más populares de Go y cómo se relacionan con tus necesidades dependiendo del proyecto.

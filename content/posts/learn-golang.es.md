---
title: "Aprende el lenguaje Go desde 0"
slug: "aprende-go-desde-cero"
date: 2022-06-26T21:00:09+02:00
draft: false
---

# Aprender Golang, el lenguaje desarrollado por Google
Este post está basado en el siguiente vídeo de mi canal. Este vídeo está en español y podrás verlo aquí:

{{< youtube rgQRfMGc6C4 >}}

Las cosas que necesitarás para seguir este tutorial son las siguientes:
* Un editor (VSCode): https://code.visualstudio.com/
* El compilador de Go: https://go.dev/dl/
* Ejemplos sencillos de Go: https://gobyexample.com/

## "Hola mundo" en Go

El primer programa que escriben todos los programadores siempre es el conocido "Hola mundo". En el lenguaje de Go se escribiría de la siguiente manera:

```go
package main

import "fmt"

func main() {
	fmt.Println("Hola mundo")
}
```

Simplemente basta con que crees un archivo de texto llamado "main.go", que copies esta pieza de código y continúes con lo siguiente que vamos a aprender.

## Cómo compilar en Go
Compilar con el lenguaje Go es muy sencillo, existen dos formas principalmente. Escribimos en la terminal `go build main.go` y se generaría un ejecutable, que podríamos ejecutar desde una terminal escribiendo `./main`.

La segunda forma y la que vamos a utilizar a lo largo del tutorial es `go run main.go`. De esta forma estaremos compilando y ejecutando el programa en una sola instrucción, que es lo que necesitamos para empezar.

Ahora hazlo tú en la terminal y deberías ver como se escribe en la consola un "Hola mundo".

## Escribir comentarios en Golang
Los comentarios son algo que tienen todos los lenguajes, y pueden tener múltiples usos, uno de ellos sería comentar el código. Conviene no abusar para que no quede un código ilegible. Se escriben con doble barra al principio `//` o si vas a escribir varias lineas, al principio pones `/*` y al final `*/`.

Este sería un ejemplo de un comentario, aunque un mal ejemplo porque ensucia el código:

```go
package main

import "fmt"

func main() {
	// Println sirve para imprimir lo que haya entre "" emn la consola 
	fmt.Println("Hola mundo")
}
```

## ¿Qué es una variable? Variables en Go
Existen varios tipos de variable en programación, los básicos son `int`, `float`, `string` y `bool`. Cada tipo de variable tiene un uso.

* int es para hacer operaciones con números enteros (sumas, restas, etc.)
* float es igual que int pero con números decimales
* string es un tipo de variable para cadenas de caracteres (ej. "Hola mundo")
* bool solo puede valer dos cosas `true` y `false`.

En el siguiente código declararemos las variables edad como int, euros como float, nombre como string, y brilla como bool.

```go
package main

import "fmt"

func main() {
	var edad int
	edad = 10
	fmt.Println("La edad que tendré dentro de 5 años es", edad+5)

	var euros float32
	euros = 10.3
	fmt.Println("Si tengo", euros, "y me gasto la mitad, tendré", euros/2)

	var nombre string
	nombre = "Arturo"
	fmt.Println("Mi nombre es", nombre)

	var brilla bool
	brilla = true
	fmt.Println("El valor de brilla es", brilla)
}
```

Cabe destacar que, a diferencia de otros lenguajes, en Go no hace fata decir el tipo de variable explícitamente, pero puede que cuando estás aprendiendo prefieras empezar declarándolas explícitamente para enterarte de lo que estás haciendo. Se puede escribir el mismo código de la siguiente manera:

```go
package main

import "fmt"

func main() {
	edad := 10
	fmt.Println("La edad que tendré dentro de 5 años es", edad+5)

	euros := 10.3
	fmt.Println("Si tengo", euros, "y me gasto la mitad, tendré", euros/2)

	nombre := "Arturo"
	fmt.Println("Mi nombre es", nombre)

	brilla := true
	fmt.Println("El valor de brilla es", brilla)
}
```

## Estructuras condicionales en Go (if y else)
Donde se suelen utilizar las variables booleanas (bool) son en este contexto. Veamos el siguiente código:

```go
package main

import "fmt"

func main() {
	var brilla bool
	brilla = true
	if brilla {
		fmt.Println("El objeto brilla")
	}
}
```

Si lo copias y lo pegas en tu archivo `main.go`, y ejecutas el siguiente comando que hemos mencionado antes `go run main.go` obtendrás como salida "El objeto brilla". 

¡Genial! Pero antes me habías dicho que bool puede valer true, o false, ¿para qué sirve la opción de false? Pues en la sintaxis del if siempre puede ir acompañado con un else. Lo que haya dentro de los corchetes `{}` será lo que se ejecutará cuando el valor de brilla sea false. Prueba con el siguiente ejemplo:

```go
package main

import "fmt"

func main() {
	var brilla bool
	brilla = false
	if brilla {
		fmt.Println("El objeto brilla")
	} else {
		fmt.Println("El objeto NO brilla")
	}
}
```

Si ahora ejecutas el comando `go run main.go` ¿qué salida obtienes?

Si ya lo has probado, verás que obtendrás "El objeto NO brilla", y esto es porque el valor de `brilla` lo hemos cambiado a `false`.

### Comparadores dentro de un if
En muchas ocasiones, no tendrás que declarar una variable booleana para usar ifs, lo que harás será usar comparaciones. Los símbolos que se utilizan en programación son 
* `>` Para indicar mayor que.
* `>=` Para indicar mayor o igual que.
* `<` Para indicar menor que.
* `<` Para indicar menor o igual que.
* `==` Para indicar si son iguales.
* `!=` Para indicar si son distintos.

```go
package main

import "fmt"

func main() {
	altura_claudio := 1.70
	altura_victor := 1.62

	if altura_claudio > altura_victor {
		fmt.Println("Claudio es más alto que Victor")
	} else {
		fmt.Println("Victor es más alto que Claudio")
	}
}
```

Pero ahora tenemos un caso más, Claudio y Victor pueden no ser uno mayor que el otro, tenemos que contemplar la opción de que ambos sean igual de altos. Para ello utilzaremor el último caso que se puede dar dentro de una estructura condicional, que es el `else if`. Sirve para expresar una opción que no está dentro de "el resto de opciones". Veamos el ejemplo:

```go
package main

import "fmt"

func main() {
	altura_claudio := 1.70
	altura_victor := 1.62

	if altura_claudio > altura_victor {
		fmt.Println("Claudio es más alto que Victor")
	} else if altura_victor > altura_claudio {
		fmt.Println("Victor es más alto que Claudio")
	} else if altura_claudio == altura_victor {
		fmt.Println("Claudio y Victor miden lo mismo")
	}
}
```

Cabe destacar que podemos usar `if`, `else if` y `else` todos en la misma sentencia. En este caso prueba a copiar el código de arriba y ejecutar de nuevo `go run main.go`. Ahora prueba a cambiar los valores de las alturas para ver los distintos resultados. Verás que lo que aparece en la terminal va cambiando.

## Arrays en Golang
Si queremos tener un conjunto de datos en todos los lenguajes, no vamos declarando las variables una a una, como hemos hecho hasta ahora altura_claudio, altura_victor, etc. Lo que haremos será utilizar un Array. La sintaxis sería la siguiente:

```go
package main

import "fmt"

func main() {
	var alturas []float32 = []float32{1.70, 1.70, 1.63, 1.65}
	fmt.Println(alturas)
}
```

Sin embargo, lo podemos simplificar como antes, usando `:=`

```go
package main

import "fmt"

func main() {
	 alturas := []float32{1.70, 1.70, 1.63, 1.65}
	fmt.Println(alturas)
}
```

Copia cualquiera de los dos códigos y ejecútalo para ver la salida en la consola.

Bien, los arrays son colecciones de datos pero ¿para qué sirven? Cuando quieras hacer operaciones con muchos datos, como un sumatorio, necesitarás recorrer todos los datos, y es por esto que necesitamos combinar los arrays con bucles.

## Bucles en Golang
Pero antes de empezar de forma complicada, vamos con lo más simple. Si queremos escribir en la consola todos los números desde el 0 hasta el 10, lo que haremos será escribir el siguiente código:

```go
package main

import "fmt"

func main() {
	for i := 0; i <= 10; i++ {
		fmt.Println(i)
	}
}
```

Este código escribirá todos los numeros del 0 al 10 en la consola al ejecutar `go run main.go`. Pero vamos a analizarlo paso por paso porque aquí han entrado muchos conceptos nuevos.

Lo que hay entre la palabra `for` y el corchete abierto `{` separado por `;` es lo siguiente:
* `i := 0` inicia el valor de la variable `i` a 0.
* `i <= 10` es una comparación de menor o igual a 10.
* `i++` incrementa el valor de `i` en 1.

Es decir, el valor inicial es 0, y ha sido guardado en una variable que se llama `i`. Y lo que hay dentro de los corchetes `{}` se ejecutará hasta que la comparación sea falsa, en este caso, hasta que `i` valga 11. Por último utilizamos el operador `++`, que no habíamos visto hasta ahora, pero es muy útil cuando usamos bucles porque incrementa la variable de uno en uno.

### Recorrer arrays en Go
Ahora volvemos con los arrays. Una vez conocemos la sintaxis en los bucles de Go, vamos a combinar este conocimiento con los arrays. Ahora queremos acceder a todos los valores dentro del array, pues lo que haremos será lo siguiente.

```go
package main

import "fmt"

func main() {
	var alturas []float32 = []float32{1.70, 1.70, 1.63, 1.65}
	fmt.Println(alturas)

	for i := 0; i < len(alturas); i++ {
		fmt.Println("La altura número", i, "es", alturas[i])
	}
}
```

Ejecútalo con `go run main.go` y verás la salida. De nuevo vemos cosas nuevas, ahora entre el `for` y el corchete `{` está la sentencia `i < len(alturas)` donde antes había un `i <= 10`. Y es que cuando usamos `len()` nos va a devolver la cantidad total de valores en el array, en este caso vemos que hay 4.

Además, al final del `Println` hemos escrito `alturas[i]`. Cuando usamos los corchetes de array `[]` le indicamos la posición a la que queremos acceder del array. En el primer caso `i` vale 0. Esto último es un poco contraintuitivo porque los humanos siempre hemos empezado a contar desde 1, pero las máquinas empiezan a contar desde 0, por lo tanto, la primera posición del array es la posición 0. Lo siguiente serían las posiciones 1, 2 y 3.

Existen más formas de utilizar el `for`, pero con esto ya sabremos lo más básico.

## Último repaso para refrescar las condiciones y los bucles
A estas alturas ya deberías saber utilizar los `if` y los `for`. Ahora vamos a combinar ambos para comprenderlos a la perfección. (min 12:50....)
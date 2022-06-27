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
	// Println sirve para imprimir lo que haya entre "" en la consola 
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

## Operaciones matemáticas en Go
En todos los lenguajes de programación, existen operaciones matemáticas, como sumar, restar, multiplicar, dividir, etc. Los símbolos que usarás para realizar estas operaciones son los siguientes:
* `+`: Sirve para sumar
* `-`: Sirve para restar
* `*`: Sirve para multiplicar
* `/`: Sirve para dividir
* `%`: Es el módulo (resto de la división)
* `^`: Sirve para elevar a la potencia

```go
package main

import "fmt"

func main() {
	var numero int = 10
	fmt.Println("El número es", numero)
	fmt.Println("El número + 1 es", numero+1)
	fmt.Println("El número - 1 es", numero-1)
	fmt.Println("El número * 2 es", numero*2)
	fmt.Println("El número / 2 es", numero/2)
	fmt.Println("El número % 2 es", numero%2)
	fmt.Println("El número ^ 2 es", numero^2)
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

### Más de una condición en un if
Puedes usar más de una condición en un if. Los operadores que se usan para añadir más condiciones a un if son:
* `&&` Para indicar que se deben cumplir ambas condiciones.
* `||` Para indicar que se debe cumplir al menos una condición.
* `!` Para indicar que se debe cumplir la condición contraria.

Veamos el siguiente ejemplo:

```go
package main

import "fmt"

func main() {
	var hora int = 12
	if hora > 8 && hora < 18 {
		fmt.Println("Estamos en horario de trabajo")
	} else {
		fmt.Println("Estamos fuera de horario de trabajo")
	}
}
```

Podemos ver que si la hora es mayor a 8 y menor a 18, el programa imprime "Estamos en horario de trabajo", es decir, que la hora ha de estar entre esos dos valores, pero no que sea exactamente esos valores. Si quisiéramos incluir las 8 y las 18 usaríamos los operadores que ya conocemos `>=` y `<=`.

La operación con `||` se entiende fácilmente porque es una operación lógica que se ejecuta si alguna de las condiciones es verdadera. A diferencia de `&&` que solo se ejecuta si ambas condiciones son verdaderas.

Pero lo que falta por saber es cómo se utiliza `!`. Veamos el siguiente ejemplo:

```go
package main

 import "fmt"
 
 func main() {
	 var dinero int = -5
	 if !(dinero > 0) {
		 fmt.Println("Tienes un saldo negativo")
	 }
 }
```

Ahora si has ejecutado este programa con `go run main.go` obtendrás que el programa imprime "Tienes un saldo negativo". Esto es porque dinero NO es mayor a cero, `dinero > 0` equivale a `false`, pero el operador `!` lo cambia a verdadero.

Obtendríamos el mismo resultado si habíamos puesto la sentencia `dinero <= 0` que es justo lo contrario a `dinero > 0`.

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

### Otro tipo de bucle en Go
Hay que aclarar que en otros lenguajes se tiene la posibilidad de utilizar `while`. En Go no existe esta palabra reservada, pero podemo hacer uso de `for` como si se tratase de un `while`.

El `while` es un bucle que se ejecuta mientras la condición es verdadera. Es parecido al if ya que solo tenemos que escribir la condición y el cuerpo del bucle.

```go
package main

import "fmt"

func main() {
	i := 0
	for i <= 10 {
		fmt.Println(i)
		i++
	}
}
```

Este programa debería hacer lo mismo que el primero que hemos escrito en la explicación de los bucles, el bucle se ejecutará 11 veces, y escribirá los números del 0 al 10.

## Último repaso para refrescar las condiciones y los bucles
A estas alturas ya deberías saber utilizar los `if` y los `for`. Ahora vamos a combinar ambos para comprenderlos a la perfección. Vamos a recorrer todos los valores de un array y decidir si es lo suficientemente alto.

```go
package main

import "fmt"

func main() {
	var alturas []float32 = []float32{1.70, 1.70, 1.63, 1.65}
	fmt.Println(alturas)

	for i := 0; i < len(alturas); i++ {
		if alturas[i] > 1.65 {
			fmt.Println("La persona número", i, "es bastante alto")
		} else {
			fmt.Println("La persona número", i, "es no es suficientemente alto")
		}
	}
}
```

Prueba a ejecutar el código con `go run main.go` y entiende la salida.

## Funciones en Go
Lo último que tienes que aprender para conocer lo más básico de la programación es las funciones. Las funciones son una forma de organizar nuestra código, y es una forma de reutilizar código. Les debemos dar un nombre que haga que el código resulte más fácilmente legible.

En Go escribiremos la palabra `func`, luego el nombre de la función, y después entre paréntesis los argumentos que recibe la función.

```go
package main

import "fmt"

func CalculaPrecioConIVA(precio float32) float32 {
	return precio * 1.21
}

func main() {
	precio := CalculaPrecioConIVA(10.0)
	fmt.Println(precio)
}
```

Como podemos ver, la función `CalculaPrecioConIVA` recibe un argumento de tipo `float32`, y devuelve un argumento de tipo `float32`. No es necesario que todas las funciones devuelvan algo, pero si lo devuelven, tenemos que poner después de cerrar el paréntesis `)` y antes de abrir el corchete `{` el tipo de variable que devuelve la función.

Si quisiéramos crear una función que no devuelva nada, sería tan sencillo como lo siguiente:

```go
package main

import "fmt"

func DiHolaA(nombre string) {
	fmt.Println("Hola", nombre)
}

func main() {
	DiHolaA("Arturo")
}
```

Dentro de las funciones podemos introducir toda la lógica que queramos, no hace falta que sea de pocas lineas como hemos hecho hasta ahora. Por ejemplo, vamos a avanzar un poquito con la dificultad y vamos a verificar si un número es primo:

Un número es primo si solo es divisible por 1 y por sí mismo.

```go
package main

import "fmt"

func EsPrimo(numero int) bool {
	for i := 2; i < numero; i++ {
		if numero%i == 0 {
			return false
		}
	}
	return true
}

func main() {
	fmt.Println("¿Es primo el número 7?", EsPrimo(7))
}
```

Esta función es algo más difícil que las primeras porque contiene más de un return, pero es perfecta para entender lo sencillo que es escribir una lógica desde palabras humanas a una función.

## Conclusión
Si has llegado hasta aquí, espero que hayas entendido todo lo que hemos visto en este post. Si no, no te preocupes, podemos volver a leerlo las veces que quieras. Para no extender más el post, lo que voy a recomendarte es que, una vez entendidos todos los conceptos que se explican, vayas a la página https://gobyexample.com/ en la cual encontrarás más ejemplos que van escalando de dificultad pero que son cosas más potentes que te ayudarán a explotar al máximo tus conocimientos en programación.

Y no olvides compartir este post a tus compañeros que estén aprendiendo Go.
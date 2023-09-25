---
title: "¿Qué es una variable?"
slug: "que-es-una-variable"
description: "El primer paso para aprender programación. ¿Cómo funcionan los programas por dentro?"
summary: "El primer paso para aprender programación. ¿Cómo funcionan los programas por dentro?"
date: 2023-09-16T20:40:24+02:00
draft: false
tags: ["mini pildoras"]
---

Una variable es una caja. En esta caja guardarás datos, accederás a ellos, y los manipularás rápidamente.

Imagina que dentro de tu caja quieres guardar tu edad.

```php
age = 25
```

Entonces en alguna parte de la RAM de tu ordenador, habrá algo parecido a esto:

```php
----
|25|
----
```

Y cuando quieras ir a esa parte desconocida de tu RAM, bastará con que en tu código pongás `age`, y mágicamente podrás ver el valor `25`.

## Tipos de datos en programación

Un dato puede ser cualquier cosa, pero los datos más sencillos son `int`, `float`, `bool` y `string`. A estos tipos de datos se les llama tipos de datos primitivos, cada lenguaje de programación los llama de una manera, pero representan lo mismo.

- **int**: número entero -> 10.2, 66.4, etc.
- **float**: número decimal -> 10.2, 66.4, etc.
- **bool**: valor booleano, es decir, verdadero o falso -> TRUE o FALSE
- **string**: cadena de texto -> "hello world", "b", etc.

Algunos lenguajes te permitirán acceder a algunos más específicos como `byte`, `char`, o punteros como `int*`, pero con los anteriores te podrás manejar con los lenguajes más nuevos.

## Buenas prácticas

Cuando empiezas a programar es muy tentador empezar a poner nombres aleatorios a las variables, porque tus programas son pequeños y no los tiene que leer nadie. NO hagas eso nunca, aunque a veces sea difícil ponerle un nombre descriptivo a una variable, siempre merece la pena.

Habitualmente tu código tendrá que ser leído por otras personas, o peor aún, **tu yo del futuro** tendrá que leer el código de **tu yo del presente**, y te aseguro que no quieres ser **tu yo del futuro** si no nombras bien tus variables.

Veamos un pequeño ejemplo.

```pseudocode
x = 10
y = x*x
```

¿Qué se supone que es `y`? Es un código tan poco descriptivo que la única forma de que sepas lo que sucede es que tengas frescos tus conocimientos de geometría. La forma correcta de nombrar las variables sería así.

```pseudocode
sideSize = 10
squareArea = sideSize*sideSize
```

Ahora entiendes lo que queríamos calcular sin ningún problema, sin que tenga que explicar el código, sólo por el nombre de las variables.

### Ejemplos con los distintos lenguajes de programación

Un concepto algo avanzado, pero que merece la pena mencionar ahora, es el de los lenguajes de programación de tipo estático y de tipo dinámico. Con los siguientes ejemplos lo entenderás perfectamente.

En el lenguaje de programación C++ tendremos que decir de qué tipo es la variable. Es de tipado estático.

```c++
#include <iostream>
#include <string>

int main() {
    int age = 25;
    std::string name = "Charles";

    std::cout << "Hello, my name is " << name << " and I'm " << age << " years old." << std::endl;

    return 0;
}
```

Mientras que el lenguaje de programación Go también es estático, pero si usamos `:=` el compilador deduce el tipo de variable por ti.

```go
package main

import (
    "fmt"
)

func main() {
    age := 25 // int
    name := "Charles" // string

    fmt.Println("Hello, my name is " + name + " and I'm " + age + " years old.")
}
```

Con JavaScript puedes olvidarte de los tipos, es un lenguaje de programación dinámico.

```js
const age = 25;
const name = "Charles";

console.log("Hello, my name is " + name + " and I'm " + age + " years old.");
```

Esto es tanto una ventaja, como un inconveniente. Puede que el código parezca más sencillo, pero a la larga será más difícil de mantener.

Y, por último, el famoso lenguaje de programación Python, que también es dinámico, aunque sutilmente diferente a JavaScript, pero esto es más avanzado.

```python
age = 25
name = "Charles"

print("Hello, my name is " + name + " and I'm " + age + " years old.")
```

---

**Disclaimer**: Con nuevas versiones de C++ puedes deducir tipos como en Go, en JavaScript no es necesario escribir const, y los mensajes de "Hello, my name is "... se pueden formatear de distintas maneras, más elegantes o sencillas. Todo lo anterior son ejemplos simples, para que se entienda fácilmente el mensaje.

---
title: "Best Practices in Go"
slug: "buenas-practicas-en-go"
description: "Conoce las mejores prácticas para tener un código mantenible en el tiempo para el lenguaje Golang"
summary: "Conoce las mejores prácticas para tener un código mantenible en el tiempo para el lenguaje Golang"
date: 2022-11-26T19:03:33+01:00
draft: false
---

## Buenas prácticas en cualquier lenguaje

Una de las cosas más importantes cuando trabajamos en equipo es seguir un mismo estilo de programación. Esto es importante porque probablemente algún compañero tendrá que ver tu código en un futuro, o tú el suyo, o lo que es peor, tú el tuyo mismo. Y querréis que ese trabajo sea lo menos tedioso posible.

Es cierto que hay algunas buenas prácticas de programación que son comunes a todos los lenguajes, como por ejemplo hacer que una función sea autodescriptiva, en definición, que el nombre de la función explique lo que haces dentro de ella, y para esto sueles necesitar que la función sea lo menos larga posible. El siguiente ejemplo, que es el del juego de la serpiente, es un ejemplo de una función que es autodescriptiva, en lugar de escribir todo el código para dibujar la serpiente, la comida, y el puntaje, lo que hice fue crear funciones que se encargaran de hacer eso, y luego llamarlas desde la función `drawGame`, que lo único que hace es dibujar el estado actual del juego.

```js
function drawGame() {
    ctx.clearRect(0, 0, GAME_WIDTH, GAME_HEIGHT);
    drawSnake();
    drawFood();
    drawScore();
    if (game.isOver) {
        drawGameOver();
    }
}
```

Otra buena práctica común a todos los lenguajes es intentar evitar el código espagueti. Esto significa que si tu código tiene un if, dentro de otro if, etc. y queda mucho espacio a la izquierda, probablemente tengas que refactorizarlo (esto aplica también a los bucles). (buscar un ejemplo para ponerlo).

### El nombrado de variables/funciones/archivos

Luego, hay otras prácticas que no son ni buenas ni malas, y que tampoco dependen del lenguaje de programación, pero sí conviene llegar a un acuerdo con el equipo antes de comenzar un proyecto. Un ejemplo de estas es la forma de llamar a las variables, funciones, y archivos, existen varias muy conocidas:

- **Pascal case:** DrawGame
- **Camel case:** drawGame
- **Snake case:** draw_game
- **Kebab case:** draw-game

Como podrás apreciar el _pascal case_ y el _camel case_ son muy parecidas, solo cambia si la primera letra será mayúscula o no. Un ejemplo de caso de uso sería: usar _pascal case_ para los nombres de las clases, y _camel case_ para los nombres de las funciones.

Por otro lado, hay gente que prefiere utilizar _snake case_, lo más habitual es en lenguajes antiguos como PHP y C, pero estoy seguro de que se utiliza en muchos equipos con lenguajes modernos.

Mientras que el _kebab case_ es habitual para nombrar archivos, ya que el guion (-) se suele utilizar para restar en programación, es un caracter reservado.

### Dar estilo con un Linter

Por otro lado, hay buenas prácticas que no dependen del lenguaje, ni son globales, son las prácticas referidas al estilo del código. Esto es más difícil de definir porque cada equipo tiene su estilo, pero de nuevo, hay que ponerlo en común para que no haya cambios constantemente en el código, y sea ilegible tanto el código como el git. Los estilos que se definen es si se usarán espacios o tabuladores, si se usa punto y coma o no, etc. Lo bueno es que si todo el equipo utiliza el mismo IDE, existen plugins (**Linter**) que te ayudan a formatear el código con un estilo concreto sin que el programador tenga que preocuparse por ello.

## Buenas prácticas de programación en Go

Lo bueno de Go es que al ser un lenguaje compilado, hay muchas malas prácticas que te corrige el propio compilador, de forma que si el código está mal escrito, se quejará hasta que lo arregles. El lenguaje Golang tiene bien definidas las reglas para minimizar esa diferencia de estilo de programación entre un programa y otro.

Una de las cosas que más me llamó la atención es que si quieres que algo (una variable, un atributo, una función) sea público, tienes que nombrarlo comenzando por mayúscula. Además creo recordar que antes no permitía usar _snake case_, pero ahora sí lo permite, aunque la mayoría de programas que veas estarán escritos con _camel/pascal case_.

Otra cosa interesante es que no te permite crear variables que no se usen, o tener funciones sin _return_. Esto es realmente útil porque ayuda a evitar futuros errores que con lenguajes interpretados (JS, PHP, etc) no verías.

Todas estas prácticas están explicadas en [este post escrito por los propios desarrolladores de Golang](https://go.dev/doc/effective_go "‌"). Pero hoy quiero ir más allá, no solo con las prácticas más básicas que te corrige el compilador.

### ¿Cómo lo hace el mejor programador?

**¡Utiliza los comentarios!** Seguro que habéis visto una definición cuando dejáis el ratón sobre `json.Marshal` esto es porque cuando crearon la librería json pusieron encima para qué servía. Si vas a programar `package`s que luego utilizarán tus compañeros, les será muy útil ver información sobre esa función (qué errores puede retornar, etc.).

**¡Jamás uses panic()!** En Go no existen las excepciones, pero panic() hace algo parecido a ellas. Cuando estamos probando algo en Go es muy cómodo utilizar panic() para parar la ejecución y encontrar el error, pero no quieres que tu programa detenga la ejecución en producción. Por eso dentro de tus funciones, siempre que lo necesites, haz return del error, nunca panic().

**¡Nunca uses _ en un error!** Si una función retorna un error, es porque hay que controlarlo, sino puede ocurrir “magia“ dentro de tu programa. Y junto a esta práctica: utiliza siempre `if err {}`, simplemente sigue el programa sin `else`, de esta manera destacas los errores y evitas el código espagueti.

**Named returns**. En Go una función puede retornar varias variables. La recomendación general es `func (n *Node) Parent() (*Node, error) {}` en lugar de `func (n *Node) Parent2() (node *Node, err error) {}`, para evitar ser demasiado “verbose“, aunque el lenguaje ya lo es de por sí. Sin embargo, si la función retorna más de dos parámetros sí que se recomienda usar named returns, ej: `func (f *Foo) Location() (lat, long float64, err error)`

**¡Variables lo más cortas posibles!** Esto aplica sobretodo para las variables dentro de un ámbito. Por ejemplo, si estás construyendo un paquete que tiene una variable que se utiliza en todo el paquete, es recomendable que sea larga y legible, mientras que si una variable solo se utiliza dentro de una función, será lo más corta posible (siempre que se entienda para qué sirve) mejor `i` que `sliceIndex`.

Muchas de estas prácticas han sido extraídas de [este otro post, escrito por los propios desarrolladores de Go](https://github.com/golang/go/wiki/CodeReviewComments "‌"). Por lo que si quieres conocer más, te recomiendo entrar a él.

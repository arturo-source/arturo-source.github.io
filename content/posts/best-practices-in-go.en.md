---
title: "Best Practices in Go"
description: "Know the best practices to have a maintainable code over time for the Golang language"
summary: "Know the best practices to have a maintainable code over time for the Golang language"
date: 2022-11-26T19:03:30+01:00
draft: false
---

## Good practices in any programming language

One of the most important things when we work as a team is to follow the same programming style. This is important because some colleague will probably have to see your code in the future, or you will have to see theirs, or what is worse, you will have to see yours. And you want that job to be as painless as possible.

It is true that there are some good programming practices that are common to all languages, such as making a function self-describing, in definition, that the name of the function explains what you do inside it, and for this you usually need that the function is as short as possible. The next example, which is the snake game, is an example of a function that is self-describing, instead of writing all the code to draw the snake, the food, and the score, what I did was create functions that will take care of doing that, and then call them from the `drawGame` function, which all it does is draw the current state of the game.

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

Otra buena práctica común a todos los lenguajes es intentar evitar el código espagueti. Esto significa que si tu código tiene un if, dentro de otro if, etc. y queda mucho espacio a la izquierda, probablemente tengas que refactorizarlo (esto aplica también a los bucles).

```go
func spaghettiCode(u User) {
    if u.Name != "" {
        if u.Address != "" {
            if u.Email != "" {
                // Do something
            } else {
                // Do something with wrong email
            }
        } else {
            // Do something with wrong address
        }
    } else {
        // Do something with wrong name
    }
}

func noSpaghettiCode(u User) {
    if u.Name == "" {
        // Do something with wrong name
        return
    }
    if u.Address == "" {
        // Do something with wrong address
        return
    }
    if u.Email == "" {
        // Do something with wrong email
        return
    }

    // Do something
}
```

### The naming of variables/functions/files

Then, there are other practices that are neither good nor bad, and that do not depend on the programming language either, but it is convenient to reach an agreement with the team before starting a project. An example of these is the way to call variables, functions, and files, there are several well-known ones:

- **Pascal case:** DrawGame
- **Camel case:** drawGame
- **Snake case:** draw_game
- **Kebab case:** draw-game

As you can see, the _pascal case_ and the _camel case_ are very similar, it only changes whether the first letter will be capitalized or not. An example use case would be: use _pascal case_ for class names, and _camel case_ for function names.

On the other hand, some people prefer to use _snake case_, most commonly in older languages like PHP and C, but I'm sure it's used in many modern language teams.

While the _kebab case_ is common for file naming, since the hyphen (-) is often used to subtract in programming, it is a reserved character.

### Styling with a Linter

On the other hand, there are good practices that do not depend on the language, nor are they global, they are the practices referred to the style of the code. This is more difficult to define because each team has its style, but again, you have to put it in common so that there are not constant changes in the code, and both the code and the git are unreadable. The styles that are defined are whether to use spaces or tabs, whether to use semicolons or not, etc. The good thing is that if the whole team uses the same IDE, there are plugins (**Linter**) that help you to format the code with a specific style without the programmer having to worry about it.

## Buenas prácticas de programación en Go

The good thing about Go is that being a compiled language, there are many bad practices that the compiler itself corrects for you, so if the code is badly written, it will complain until you fix it. The Golang language has well-defined rules to minimize this difference in programming style between one program and another.

One of the things that caught my attention the most is that if you want something (a variable, an attribute, a function) to be public, you have to name it starting with a capital letter. Also, I seem to remember that before it didn't allow the use of _snake case_, but now it does, although most of the programs you'll see will be written with _camel/pascal case_.

Another cool thing is that it doesn't allow you to create variables that aren't used, or have functions without _return_. This is really useful because it helps to avoid future errors that with interpreted languages (JS, PHP, etc) you wouldn't see.

All these practices are explained in [this post written by the Golang developers](https://go.dev/doc/effective_go "‌"). But today I want to go further, not only with the most basic practices that the compiler corrects for you.

### How does the best programmer should do it?

**Use the comments!** I'm sure you've seen a definition when you mouse over `json.Marshal` this is because when they created the json library they put what it was for on top of it. If you are going to write `package`s that your colleagues will later use, it will be very useful for them to see information about that function (what errors it can return, etc.).

**Never use panic()!** In Go there are no exceptions, but panic() does something similar to them. When you're testing something in Go it's very convenient to use panic() to stop execution and find the error, but you don't want your program to stop execution in production. That's why within your functions, whenever you need it, return error, never panic().

**Never use _ in an error!** If a function returns an error, it's because it needs to be handled, otherwise “magic” can happen inside your program. And next to this practice: always use `if err {}`, just follow the program without `else`, this way you highlight errors and avoid code spaghetti.

**Named returns**. In Go, a function can return several variables. The general recommendation is `func (n *Node) Parent() (*Node, error) {}` instead of `func (n *Node) Parent2() (node *Node, err error) {}`, to avoid be too "verbose", although the language already is in itself. However, if the function returns more than two parameters, it is recommended to use named returns, eg: `func (f *Foo) Location() (lat, long float64, err error)`

**Variables as short as possible!** This applies above all to variables within a scope. For example, if you are building a package that has a variable that is used throughout the package, it is recommended that it be long and readable, whereas if a variable is only used within a function, it should be as short as possible (as long as it is used). understand what it is for) better `i` than `sliceIndex`.

Many of these practices have been taken from [this other post, written by the Go developers](https://github.com/golang/go/wiki/CodeReviewComments "‌"). So if you want to know more, I recommend you enter it.

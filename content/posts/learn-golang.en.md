---
title: "Learn Golang from base"
description: "What's a variable? How to do an if? And a loop? Learn all the basics you need to start programming, in a easy language as Go."
summary: "What's a variable? How to do an if? And a loop? Learn all the basics you need to start programming, in a easy language as Go."
date: 2022-06-26T20:59:13+02:00
draft: false
---

# Learn Golang, the language developed by Google

This post is based on the following video on my channel. This video is in Spanish, but if you know Spanish, you can see it here:

{{< youtube rgQRfMGc6C4 >}}

But dont worry if you dont know Spanish, I'll let all explained here. The things you will need to follow this tutorial are the following:

- An editor (VSCode): https://code.visualstudio.com/
- The Go compiler: https://go.dev/dl/

## "Hello world" in Go

The first program that all programmers write is always the well-known "Hello world". In Go language it would be written as follows:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello world")
}
```

You just need to create a text file called "main.go", copy this piece of code and continue with the next thing we are going to learn.

## How to compile in Go

Compiling with the Go language is very simple, there are two main ways. We typed in the terminal `go build main.go` and an executable would be generated, which we could run from a terminal by typing `./main`.

The second way and the one we will use throughout the tutorial is `go run main.go`. In this way we will be compiling and executing the program in a single instruction, which is what we need to start with.

Now do it yourself in the terminal and you should see a "Hello world" being written to the console.

## Write comments in Golang

Comments are something that all languages have, and they can have multiple uses, one of them would be to comment the code. It is advisable not to abuse so that there is no unreadable code. They are written with a double slash at the beginning `//` or if you are going to write several lines, at the beginning you put `/*` and at the end `*/`.

This would be an example of a comment, although a bad example because it messes up the code:

```go
package main

import "fmt"

func main() {
// Println is used to print whatever is between "" to the console
    fmt.Println("Hello world")
}
```

## What is a variable? Variables in Go

There are several types of variable in programming, the basic ones are `int`, `float`, `string` and `bool`. Each type of variable has a use.

- int is for doing operations with integers (addition, subtraction, etc.)
- float is the same as int but with decimal numbers
- string is a variable type for character strings (eg "Hello world")
- bool can only be two things `true` and `false`.

In the following code we will declare the variables age as int, euros as float, name as string, and shine as bool.

```go
package main

import "fmt"

func main() {
    var int age
    age = 10
    fmt.Println("I'll be", age+5, "in 5 years")

    var euros float32
    euros = 10.3
    fmt.Println("If I have", euros, "and I spend half, I will have", euros/2)

    var name string
    name = "Arturo"
    fmt.Println("My name is", name)

    var shines bool
    shines = true
    fmt.Println("The value of shines is", shines)
}
```

It should be noted that, unlike other languages, it is not necessary to say the type of a variable explicitly in Go, but when you are learning you may prefer to start by declaring them explicitly to find out what you are doing. The same code can be written as follows:

```go
package main

import "fmt"

func main() {
    age := 10
    fmt.Println("My age in 5 years is", age+5)

    euros := 10.3
    fmt.Println("If I have", euros, "and I spend half, I will have", euros/2)

    name := "Arturo"
    fmt.Println("My name is", name)

    shines := true
    fmt.Println("The value of shines is", shines)
}
```

## Math operations in Go

All programming languages have mathematical operations, such as adding, subtracting, multiplying, dividing, etc. The symbols that you will use to perform these operations are the following:

- `+`: Used to add
- `-`: Used to subtract
- `*`: Used to multiply
- `/`: Used to divide
- `%`: Is the module (rest of the division)
- `^`: Used to raise to power

```go
package main

import "fmt"

func main() {
var int number = 10
    fmt.Println("Number is", number)
    fmt.Println("The number + 1 is", number+1)
    fmt.Println("Number - 1 is", number-1)
    fmt.Println("Number * 2 is", number*2)
    fmt.Println("The number / 2 is", number/2)
    fmt.Println("The number % 2 is", number%2)
    fmt.Println("Number ^ 2 is", number^2)
}
```

## Conditional structures in Go (if and else)

Boolean variables (bool) are often used in this context. Let's look at the following code:

```go
package main

import "fmt"

func main() {
    var shines bool
    shines = true
    if shines {
        fmt.Println("The object shines")
    }
}
```

If you copy and paste it into your `main.go` file, and run the following command mentioned above `go run main.go` you will get "The object shines" as output.

Cool! But before you told me that bool can be true, or false, what is the false option for? Well, in the if syntax it can always be accompanied by an else. Whatever is inside the `{}` brackets will be what will be executed when the value of shine is false. Try the following example:

```go
package main

import "fmt"

func main() {
    var shines bool
    shines = false
    if shines {
        fmt.Println("The object shines")
    } else {
        fmt.Println("Object DOES NOT shine")
    }
}
```

If you now run the command `go run main.go` what output do you get?

If you've already tried it, you'll see that you'll get "Object DOES NOT shine", and this is because the value of `glows` has been changed to `false`.

### Comparators inside an if

In many cases, you will not have to declare a boolean variable to use ifs, what you will do is use comparisons. Symbols used in programming are:

- `>` To indicate greater than.
- `>=` To indicate greater than or equal to.
- `<` To indicate less than.
- `<` To indicate less than or equal to.
- `==` To indicate if they are equal.
- `!=` To indicate if they are different.

```go
package main

import "fmt"

func main() {
    claudio_height := 1.70
    victor_height := 1.62

    if claudio_height > victor_height {
        fmt.Println("Claudio is taller than Victor")
    } else {
        fmt.Println("Victor is taller than Claudio")
    }
}
```

But now we have one more case, Claudio and Victor may not be one higher than the other, we have to consider the option that both are equally tall. To do this, use the last case that can occur within a conditional structure, which is the `else if`. It is used to express an option that is not included in "the rest of the options". Let's look at the example:

```go
package main

import "fmt"

func main() {
    claudio_height := 1.70
    victor_height := 1.62

    if claudio_height > victor_height {
        fmt.Println("Claudio is taller than Victor")
    } else if victor_height > claudio_height {
        fmt.Println("Victor is taller than Claudio")
    } else if claudio_height == victor_height {
        fmt.Println("Claudio and Victor are the same height")
    }
}
```

Note that we can use `if`, `else if` and `else` all in the same statement. In this case try copying the above code and running `go run main.go` again. Now try changing the height values ​​to see the different results. You will see that what appears in the terminal is changing.

### More than one condition in an if

You can use more than one condition in an if. The operators used to add more conditions to an if are:

- `&&` To indicate that both conditions must be met.
- `||` To indicate that at least one condition must be met.
- `!` To indicate that the opposite condition must be met.

Let's look at the following example:

```go
package main

import "fmt"

func main() {
    var hour int = 12
    if hour > 8 && hour < 18 {
        fmt.Println("We are in business hours")
    } else {
        fmt.Println("We are out of business hours")
    }
}
```

We can see that if the time is greater than 8 and less than 18, the program prints "We are in working hours", that is, the time must be between those two values, but not exactly those values. If we wanted to include all 8 and 18 we would use the familiar `>=` and `<=` operators.

The operation with `||` is easily understood because it is a logical operation that is executed if any of the conditions is true. Unlike `&&` which is only executed if both conditions are true.

But what remains to be seen is how `!` is used. Let's look at the following example:

```go
package main

  import "fmt"

  func main() {
var money int = -5
if !(money > 0) {
fmt.Println("You have a negative balance")
}
  }
```

Now if you have run this program with `go run main.go` you will get the program to print "You have a negative balance". This is because money is NOT greater than zero, `money > 0` equals `false`, but the `!` operator changes it to `true`.

We would get the same result if we had put the statement `money <= 0` which is just the opposite of `money > 0`.

## Arrays in Golang

If we want to have a set of data in all languages, we do not declare the variables one by one, as we have done so far claudio_height, victor_height, etc. What we will do is use an Array. The syntax would be the following:

```go
package main

import "fmt"

func main() {
    var heights []float32 = []float32{1.70, 1.70, 1.63, 1.65}
    fmt.Println(heights)
}
```

However, we can simplify it as before, using `:=`

```go
package main

import "fmt"

func main() {
    heights := []float32{1.70, 1.70, 1.63, 1.65}
    fmt.Println(heights)
}
```

Copy either of the two codes and run it to see the output in the console.

Well, arrays are collections of data, but what are they for? When you want to do operations with a lot of data, like a summation, you will need to iterate through all the data, and this is why we need to combine arrays with loops.

## Loops in Golang

Before starting in a complicated way, let's go with the simplest. If we want to write to the console all the numbers from 0 to 10, what we will do is write the following code:

```go
package main

import "fmt"

func main() {
    for i := 0; i <= 10; i++ {
        fmt.Println(i)
    }
}
```

This code will write all the numbers from 0 to 10 to the console when executing `go run main.go`. But we are going to analyze it step by step because many new concepts have entered here.

What is between the word `for` and the open bracket `{` separated by `;` is the following:

- `i := 0` initializes the value of the variable `i` to 0.
- `i <= 10` is a comparison of less than or equal to 10.
- `i++` increments the value of `i` by 1.

That is, the initial value is 0, and it has been stored in a variable called `i`. And what's inside the `{}` brackets will be executed until the comparison is false, in this case, until `i` equals 11. Finally we use the `++` operator, which we haven't seen before, but it is very useful when we use loops because it increments the variable one by one.

### Loop through arrays in Go

Now we return to the arrays. Once we know the syntax in Go loops, we are going to combine this knowledge with arrays. Now we want to access all the values inside the array, because what we will do is the following.

```go
package main

import "fmt"

func main() {
    var heights []float32 = []float32{1.70, 1.70, 1.63, 1.65}
    fmt.Println(heights)

    for i := 0; i < len(heights); i++ {
        fmt.Println("The height number", i, "is", heights[i])
    }
}
```

Run it with `go run main.go` and you will see the output. Again we see new things, now between the `for` and the bracket `{` is the statement `i < len(heights)` where before there was an `i <= 10`. And it is that when we use `len()` it will return the total amount of values in the array, in this case we see that there are 4.

Also, at the end of the `Println` we have written `heights[i]`. When we use the array brackets `[]` we indicate the position we want to access from the array. In the first case, `i` is equal to 0. The latter is a bit counterintuitive because humans have always started counting from 1, but machines start counting from 0, so the first position in the array is position 0. Next would be positions 1, 2 and 3.

There are more ways to use `for`, but with this we will know the basics.

### Another type of loop in Go

It should be noted that in other languages it is possible to use `while`. In Go there is no such keyword, but we can use `for` as if it were a `while`.

The `while` is a loop that runs while the condition is true. It is similar to if since we only have to write the condition and the body of the loop.

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

This program should do the same thing as the first one we wrote in the loop explanation, the loop will run 11 times, writing the numbers 0 to 10.

## Last review to refresh conditions and loops

By now you should know how to use `if`s and `for`s. Now we are going to combine both to understand them perfectly. Let's iterate through all the values in an array and decide if it's high enough.

```go
package main

import "fmt"

func main() {
    var heights []float32 = []float32{1.70, 1.70, 1.63, 1.65}
    fmt.Println(heights)

    for i := 0; i < len(heights); i++ {
        if heights[i] > 1.65 {
            fmt.Println("The number person", i, "is quite tall")
        } else {
            fmt.Println("The person number", i, "is not tall enough")
        }
    }
}
```

Try running the code with `go run main.go` and understand the output.

## Functions in Go

The last thing you have to learn to know the basics of programming is functions. Functions are a way of organizing our code, and it's a way of reusing code. We must give them a name that makes the code more easily readable.

In Go we will write the word `func`, then the name of the function, and then in parentheses the arguments that the function receives.

```go
package main

import "fmt"

func CalculatePriceWithVAT(price float32) float32 {
    return price * 1.21
}

func main() {
    price := CalculatePriceWithVAT(10.0)
    fmt.Println(price)
}
```

As we can see, the `CalculatePriceWithVAT` function receives an argument of type `float32`, and returns an argument of type `float32`. Not all functions need to return something, but if they do, we need to put after the closing parenthesis `)` and before the opening bracket `{` the type of variable that the function returns.

If we wanted to create a function that returns nothing, it would be as simple as the following:

```go
package main

import "fmt"

func SayHelloTo(string name) {
    fmt.Println("Hello", name)
}

func main() {
    SayHelloTo("Arturo")
}
```

Within the functions we can introduce all the logic we want, it does not have to be a few lines as we have done so far. For example, let's go a little further with the difficulty and check if a number is prime:

A number is prime if it is only divisible by 1 and itself.

```go
package main

import "fmt"

func IsPrime(int number) bool {
    for i := 2; i < number; i++ {
        if number%i == 0 {
            return false
        }
    }
    return true
}

func main() {
    fmt.Println("Is number 7 prime?", IsPrime(7))
}
```

This function is somewhat more difficult than the first ones because it contains more than one return, but it is perfect to understand how easy it is to write logic from human words to a function.

## Conclusion

If you have come this far, I hope you have understood everything we have seen in this post. If not, don't worry, we can reread it as many times as you want. In order not to extend the post further, what I am going to recommend is that, once you understand all the concepts that are explained, go to the page https://gobyexample.com/ where you will find more examples that increase in difficulty but They are more powerful things that will help you make the most of your programming knowledge.

And don't forget to share this post with your fellow Go learners.

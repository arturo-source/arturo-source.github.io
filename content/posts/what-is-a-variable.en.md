---
title: "What is a variable?"
description: "The first step to learn programming. How do the programs work inside?"
summary: "The first step to learn programming. How do the programs work inside?"
date: 2023-09-16T20:40:18+02:00
draft: false
tags: ["tiny pills"]
---

A variable is a box. In this box you will store data, access it, and manipulate it quickly.

Imagine that inside your box you want to store your age.

```php
age = 25
```

So somewhere in your computer's RAM, there will be something like this:

```php
----
|25|
----
```

And whenever you want to go to that unknown part of your RAM, all you have to do is put `age` in your code, and magically you will be able to see the value `25`.

## Data types in programming

A piece of data can be anything, but the simplest pieces of data are `int`, `float`, `bool` and `string`. These data types are called primitive data types, each programming language calls them in a different way, but they represent the same thing.

- **int**: integer -> 10.2, 66.4, etc.
- **float**: decimal number -> 10.2, 66.4, etc.
- **bool**: boolean value, i.e. true or false -> TRUE or FALSE
- **string**: text string -> "hello world", "b", etc.

Some languages will allow you to access more specific ones like `byte`, `char`, or pointers like `int*`, but with the above you will be able to handle the newer languages.

## Good practices

When you start programming it is very tempting to start giving random names to variables, because your programs are small and no one has to read them. DON'T ever do that, although it is sometimes difficult to give a variable a descriptive name, it is always worth it.

Usually your code will have to be read by other people, or worse still, **your future self** will have to read the code of **your present self**, and I assure you that you do not want to be **your current self**. future\*\* if you don't name your variables correctly.

Let's look at a small example.

```pseudocode
x = 10
y = x*x
```

What is `y` supposed to be? It is such a non-descriptive code that the only way for you to know what is happening is if you have fresh knowledge of geometry. The correct way to name the variables would be like this.

```pseudocode
sideSize = 10
squareArea = sideSize*sideSize
```

Now you understand what we wanted to calculate without any problem, without having to explain the code, just by the name of the variables.

### Examples with different programming languages

A somewhat advanced concept, but worth mentioning now, is that of statically typed and dynamically typed programming languages. With the following examples you will understand it perfectly.

In the C++ programming language we will have to say what type the variable is. It is statically typed.

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

While Go programming language is also static, but if we use `:=` the compiler deduces the type of variable for you.

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

With JavaScript you can forget about types, it is a dynamic programming language.

```js
const age = 25;
const name = "Charles";

console.log("Hello, my name is " + name + " and I'm " + age + " years old.");
```

This is both an advantage and a disadvantage. The code may seem simpler, but in the long run it will be more difficult to maintain.

And finally, the famous Python programming language, which is also dynamic, although subtly different from JavaScript, but this is more advanced.

```python
age = 25
name = "Charles"

print("Hello, my name is " + name + " and I'm " + age + " years old.")
```

---

**Disclaimer**: With new versions of C++ you can deduce types like in Go, in JavaScript it is not necessary to write const, and "Hello, my name is "... messages can be formatted in different ways, more elegant or simple. All of the above are simple examples, so that the message is easily understood.

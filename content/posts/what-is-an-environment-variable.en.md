---
title: "What is an Environment Variable?"
description: "What is an environment variable for? How to use it? All in one minute."
summary: "What is an environment variable for? How to use it? All in one minute."
date: 2023-09-18T20:12:27+02:00
draft: false
tags: ["tiny pills"]
---

An environment variable is global data, stored in the operating system, that can be easily read by any program.

Typically you will use it to manage data that changes between a 'production' environment, and a 'development' environment. So environment variables allow you to change the behavior of your program, without having to change the code.

## What data is stored in Environment Variables?

You can really save any type of primitive data (you learned about it in [the variables post](/posts/what-is-a-variable/)), in a way as simple as this:

```text
MY_NAME=Arturo
YT_CHANNEL_URL=https://www.youtube.com/@arturosource
MAX_TIMEOUT=10
```

Generally, you will write the environment variable in capital letters, followed by the name of the variable with a `=`, and finally, the value without spaces at the beginning or at the end. But there are several ways to let a program know which variables to use. I will focus on Linux, which is where you will commonly program, and deploy your programs.

1. At the operating system level: it is as simple as editing the `/etc/environment` file, and follows the style that appears above. Use the `nano` text editor, for example, and you will need permissions to edit the file. So the command will be `sudo nano /etc/environment`.
2. At the program level: If you run an application from the terminal, you can assign as many variables as you want before execution. Using the previous example, you could write a command like this `MY_NAME=Arturo MAX_TIMEOUT=10 bash ./coolbashscript`.
3. In a file that your application will read: in modern languages you will have a library with which you can read a file as if it had environment variables, commonly it will be called `.env`. You will have to look for the corresponding library for your programming language, and that's it.

### What data is most common to store?

Store any data that you think is relevant, with experience you will know which ones should be stored and which ones should not. But here are some examples to get you started.

- **System paths**: Tells a program where to look for executable files, libraries, resources, or configuration files.
- **Language and region**: Determines the regional and language settings used by your application.
- **Authentication data**: Stores authentication credentials, such as API keys or passwords.
- **Settings**: Customize fields such as the listening port, domain name, or debugging level.

## How to use environment variables in your program?

Finally, some examples of how environment variables are read in the most common languages.

With Golang, importing `os`.

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    ytChannel := os.Getenv("YT_CHANNEL_URL")
    fmt.Printf("Valor de YT_CHANNEL_URL: %s\n", ytChannel)
}
```

With JavaScript, using the global variable `process`.

```js
const ytChannel = process.env.YT_CHANNEL_URL;
console.log(`Valor de YT_CHANNEL_URL: ${ytChannel}`);
```

With PHP, the global function `getenv`.

```php
$ytChannel = getenv("YT_CHANNEL_URL");
echo "Valor de YT_CHANNEL_URL: " . $ytChannel;
```

With Python, setting `os` too.

```python
import os

ytChannel = os.getenv("YT_CHANNEL_URL")
print(f"Valor de YT_CHANNEL_URL: {ytChannel}")
```

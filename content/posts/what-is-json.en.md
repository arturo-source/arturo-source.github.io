---
title: "What is JSON?"
description: "Learn what JSON is and what it is used for in a minute."
summary: "Learn what JSON is and what it is used for in a minute."
date: 2023-09-10T18:07:29+02:00
draft: false
tags: ["tiny pills"]
---

JSON is a lightweight, human-readable data exchange standard.

JSON (JavaScript Object Notation) uses a structure of key-value pairs to represent information independently of the programming language. This allows you to store, read, send and receive data, regardless of the software, protocol, or programming language you use.

## Why choose JSON?

Of course, there are other standards that allow you to do one, some, all or more functions than JSON (including XML, YAML, protobuf, etc.), but the reasons to choose JSON are:

- **Readability**: JSON uses human-readable syntax. It is easy to read and write
- **Easy to use**: All programming languages have a library to parse JSON.
- **Adoption**: Most of the APIs that you are going to consume will respond in JSON.

## JSON Syntax

Typically, you will find JSON with the following format, representing, for example, a person:

```json
{
  "name": "Alex",
  "surname": "Walker",
  "age": 30,
  "height": 1.8,
  "lovesPasta": true,
  "interests": ["sport", "cook", "films"]
}
```

But you can also find a set of people (I will remove fields from the JSON for simplicity):

```json
[
  {
    "name": "Alex",
    "age": 30
  },
  {
    "name": "John",
    "age": 27
  },
  {
    "name": "Sophia",
    "age": 32
  }
]
```

A **common error**: remember that the last field of the object `{ ... }` and the array `[ ... ]` do not have a comma.

## Examples with code

Below are some examples of how to read JSON with the most common languages. The data could be obtained from an API call, or by reading a `.json` file. Depending on the language and the situation, we will do different error handling, but these are very simple examples.

### Golang

```go
package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    jsonData := `{
        "name": "Alex",
        "surname": "Walker",
        "age": 30,
        "height": 1.8,
        "lovesPasta": true,
        "interests": ["sport", "cook", "films"]
    }`

    type Person struct {
        Name       string    `json:"name"`
        Surname    string    `json:"surname"`
        Age        int       `json:"age"`
        Height     float64   `json:"height"`
        LovesPasta bool      `json:"lovesPasta"`
        Interests  []string  `json:"interests"`
    }

    var person Person
    if err := json.Unmarshal([]byte(jsonData), &person); err != nil {
        fmt.Println(err)
        return
    }

    fmt.Printf("Name: %s\nSurname: %s\nAge: %d\nHeight: %.2f\nLoves Pasta: %v\nInterests: %v\n", person.Name, person.Surname, person.Age, person.Height, person.LovesPasta, person.Interests)
}
```

### JavaScript

```javascript
var jsonData = `{
        "name": "Alex",
        "surname": "Walker",
        "age": 30,
        "height": 1.8,
        "lovesPasta": true,
        "interests": ["sport", "cook", "films"]
    }`;

var person = JSON.parse(jsonData);

console.log("Name: " + person.name);
console.log("Surname: " + person.surname);
console.log("Age: " + person.age);
console.log("Height: " + person.height);
console.log("Loves Pasta: " + (person.lovesPasta ? "Yes" : "No"));
console.log("Interests: " + person.interests.join(", "));
```

### PHP

```php
<?php
$jsonData = '{
    "name": "Alex",
    "surname": "Walker",
    "age": 30,
    "height": 1.8,
    "lovesPasta": true,
    "interests": ["sport", "cook", "films"]
}';

$person = json_decode($jsonData, true);
if ($person === null) {
    echo "Error al decodificar el JSON\n";
} else {
    echo "Name: " . $person["name"] . "\n";
    echo "Surname: " . $person["surname"] . "\n";
    echo "Age: " . $person["age"] . "\n";
    echo "Height: " . $person["height"] . "\n";
    echo "Loves Pasta: " . ($person["lovesPasta"] ? "Yes" : "No") . "\n";
    echo "Interests: " . implode(", ", $person["interests"]) . "\n";
}
?>
```

### Python

```python
import json

jsonData = '''
{
    "name": "Alex",
    "surname": "Walker",
    "age": 30,
    "height": 1.8,
    "lovesPasta": true,
    "interests": ["sport", "cook", "films"]
}
'''

person = json.loads(jsonData)

print("Name:", person["name"])
print("Surname:", person["surname"])
print("Age:", person["age"])
print("Height:", person["height"])
print("Loves Pasta:", "Yes" if person["lovesPasta"] else "No")
print("Interests:", ", ".join(person["interests"]))
```

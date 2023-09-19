---
title: "¿Qué es JSON?"
slug: "que-es-json"
description: "Aprende qué es y para qué sirve JSON en un minuto."
summary: "Aprende qué es y para qué sirve JSON en un minuto."
date: 2023-09-10T18:10:11+02:00
draft: false
tags: ["mini pildoras"]
---

JSON es un estándar de intercambio de datos ligero y legible por humanos.

JSON (JavaScript Object Notation) utiliza una estructura de pares clave-valor para representar información de manera independiente del lenguaje de programación. Esto permite almacenar, leer, enviar y recibir datos, independientemente del software, protocolo, o lenguaje de programación que uses.

## ¿Por qué elegir JSON?

Por supuesto, existen otros estándares que permiten hacer una, algunas, todas o más funciones que JSON (entre ellos XML, YAML, protobuf, etc.), pero los motivos para elegir JSON son:

- **Legibilidad**: JSON utiliza una sintaxis legible por humanos. Es fácil de leer y escribir
- **Fácil de usar**: Todos los lenguajes de programación disponen de una librería para parsear JSON.
- **Adopción**: La mayoría de APIs que vayas a consumir, responderán en JSON.

## Sintaxis de JSON

Habitualmente, te encontrarás con JSON con el siguiente formato, representando, por ejemplo, una persona:

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

Pero también puedes encontrarte a un conjunto de personas (eliminaré campos del JSON para simplificar):

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

Un **error** común: recuerda que el último campo del objeto `{ ... }` y del array `[ ... ]`, no llevan coma.

## Ejemplos con código

A continuación, algunos ejemplos de cómo leer un JSON con los lenguajes más comunes. Los datos se podrían obtener de una llamada a una API, o leyendo un fichero `.json`. Dependiendo del lenguaje, y la situación, haremos un manejo de errores distinto, pero estos son ejemplos muy sencillos.

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
var jsonData = 
    `{
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

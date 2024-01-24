---
title: "¿Qué es YAML?"
slug: "que-es-yaml"
description: "Aprende qué es cómo usar YAML en unos minutos."
summary: "Aprende qué es cómo usar YAML en unos minutos."
date: 2024-01-23T17:25:09+01:00
draft: false
tags: ["mini pildoras"]
---

YAML es un formato de datos para ficheros de configuración, creado para ser legible por humanos.

YAML (YAML Ain't Markup Language) es un acrónimo recursivo, como muchos otros en el mundo de la informática. YAML fue diseñado pensando en que sea fácilmente legible por humanos, cansado de usar lenguajes de marcado (HTML, XML, etc.) para ficheros de configuración. Acepta todo tipo de datos: strings, enteros, decimales, booleanos, nulos, listas y objetos.

## ¿Por qué aprender YAML?

Realmente puedes aprender YAML en una hora, y es un formato bastante popular, por lo que es un conocimiento que merece la pena tener. Algunos ejemplos de herramientas que usan YAML son Kubernetes, Docker, o Hugo, entre otros. Incluso, si quieres utilizarlo en tu propio proyecto, todos los lenguajes conocidos tienen una implementación para poder usarlo fácilmente (puedes verlo en la [página oficial](https://yaml.org/)).

## ¿Cómo es la sintaxis de YAML?

Empecemos con los datos simples: string, int, float y bool.

```yaml
fullName: Arturo Source
age: 25
height: 1.81
hasHouse: true
```

Como ves, no es necesario utilizar comillas para escribir un string. A no ser que quieras indicar explícitamente que es un string. Por ejemplo, si pusiera `age: '25'`, entonces la edad pasaría de ser un int a un string. O también si quieres utilizar caracteres especiales (`:`, `[`, `]`, `?`, `&`, entre otros).

Además, si necesitas escribir un comentario tienes que usar el caracter `#`, y para valores nulos `null`.

```yaml
fullName: Arturo Source # real??
# age: 25
height: null
```

También, puede que necesites un string multilinea, esto es fácil utilizando el caracter `|`.

```yaml
book:
  title: The Great Gatsby
  abstract: |
    "The Great Gatsby" is a classic novel written by F. Scott Fitzgerald.
    Set in the roaring twenties, the story explores themes of wealth, love, and the American Dream through the lens of the mysterious Jay Gatsby. 
    The narrative is narrated by Nick Carraway, a young man who becomes entangled in the lives of Gatsby and his wealthy social circle in Long Island.
  releaseYear: 1925
```

### Arrays y objetos en YAML

YAML trata de mantenerse lo más legible posible, por eso utiliza 2 o 4 espacios (**NUNCA tabulaciones**) para crear arrays u objetos. De esta forma, sabes de un vistazo a qué pertenecen.

```yaml
# my favourite fruits
fruits:
  - apple
  - banana
  - orange
```

```yaml
person:
  name: Arturo
  age: 25
```

Y si quieres un array de objetos, solo pondrás un guión `-` en el primer atributo del objeto.

```yaml
people:
  - name: Arturo
    age: 25
  - name: John
    age: 22
  - name: Lucy
    age: 28
```

Y los objetos permiten tantas anidaciones como necesites.

```yaml
employee:
  name: Charles
  department:
    name: Marketing
    charge: boss
```

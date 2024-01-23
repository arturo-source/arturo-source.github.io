---
title: "What is YAML?"
description: ""
summary: ""
date: 2024-01-23T17:25:06+01:00
draft: false
tags: ["tiny pills"]
---

YAML is a data format for configuration files, created to be human readable.

YAML (YAML Ain't Markup Language) is a recursive acronym, like many others in the computer world. YAML was designed to be easily readable by humans, tired of using markup languages (HTML, XML, etc.) for configuration files. It accepts all types of data: strings, integers, decimals, booleans, nulls, lists and objects.

## Why learning YAML?

You can really learn YAML in one hour, and it's quite a popular format, so it's a worthwhile knowledge to have. Some examples of tools that use YAML are Kubernetes, Docker, or Hugo, among others. Even, if you want to use it in your own project, all known languages have an implementation to use it easily (you can see it in the [official website](https://yaml.org/)).

## How is the syntax of YAML?

Let's start with simple data: string, int, float and bool.

```yaml
fullName: Arturo Source
age: 25
height: 1.81
hasHouse: true
```

As you can see, it is not necessary to use quotation marks to write a string. Unless you want to explicitly indicate that it is a string. For example, if you put `age: '25'`, then the age would change from an int to a string. Or if you want to use special characters (`:`, `[`, `]`, `?`, `&`, among others).

Also, if you need to write a comment you have to use the `#` character, and for null values `null`.

```yaml
fullName: Arturo Source # real??
# age: 25
height: null
```

Also, you may need a multiline string, this is easy using the `|` character.

```yaml
book:
  title: The Great Gatsby
  abstract: |
    "The Great Gatsby" is a classic novel written by F. Scott Fitzgerald.
    Set in the roaring twenties, the story explores themes of wealth, love, and the American Dream through the lens of the mysterious Jay Gatsby. 
    The narrative is narrated by Nick Carraway, a young man who becomes entangled in the lives of Gatsby and his wealthy social circle in Long Island.
  releaseYear: 1925
```

### Arrays and objects in YAML

YAML tries to stay as readable as possible, so it uses 2 or 4 spaces (**NEVER tabs**) to create arrays or objects. This way, you know at a glance what they belong to.

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

And if you want an array of objects, you just put a dash `-` in the first attribute of the object.

```yaml
people:
  - name: Arturo
    age: 25
  - name: John
    age: 22
  - name: Lucy
    age: 28
```

And objects allow as much nesting as you need.

```yaml
employee:
  name: Charles
  department:
    name: Marketing
    charge: boss
```

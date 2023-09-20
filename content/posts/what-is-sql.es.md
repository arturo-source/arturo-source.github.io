---
title: "¿Qué es SQL?"
slug: "que-es-sql"
description: "Aprende SQL en unos pocos minutos."
summary: "Aprende SQL en unos pocos minutos."
date: 2023-09-12T18:26:06+02:00
draft: false
tags: ["mini pildoras"]
---

SQL es un lenguaje utilizado para consultar y gestionar bases de datos relacionales.

SQL (Structured Query Language) fue desarrollado en la década de 1970 y se ha convertido en un estándar en la gestión y manipulación de bases de datos relacionales. Utilizarás SQL siempre con un motor de base de datos, que es una pieza de Software que contiene la lógica para manipular los datos. Es muy común utilizar un motor de base de datos en programación, porque nos permite no tener que programar la lógica de datos desde cero.

## Sintaxis de SQL

Cada motor de base de datos (MySQL, MariaDB, PostgreSQL, SQLite, etc.) tiene añadidos para hacer más fácil el tratamiento de los datos, pero todos tienen en común la sintaxis básica de SQL. Las operaciones comunes que querrás hacer en una base de datos son:

### Crear una tabla

Lo primero que querrás hacer es crear una tabla para poder insertar datos, borrarlos, etc.

```sql
CREATE TABLE Clients (
    ID INT PRIMARY KEY,
    FullName VARCHAR(50),
    Email VARCHAR(100)
);
```

Esta tabla va a tener:

- ID (identificador irrepetible, por eso es PRIMARY KEY, y es un número entero, por eso INT).
- FullName (cadena de texto, por eso VARCHAR, de longitud máxima 50 caracteres).
- Email (cadena de texto, esta vez de longitud 100).

### Borrar una tabla

Esta tabla también se puede borrar.

```sql
DROP TABLE Clients;
```

### Consultar una tabla

```sql
SELECT * FROM Clients;
```

También puedes elegir qué campos quieres obtener, `*` significa **todos**.

```sql
SELECT ID, FullName FROM Clients;
```

### Insertar datos en una tabla

```sql
INSERT INTO Clients (ID, FullName, Email)
VALUES (1, 'Barnes Smith', 'bsmith@mail.com');
```

### Actualizar datos de una tabla

Es muy importante la cláusula WHERE en una actualización. Si no usas WHERE, todas las filas de la tabla se actualizarán.

```sql
UPDATE Clients
SET Email = 'newbsmith@mail.com'
WHERE ID = 1;
```

### Borrar una fila de una tabla

Cuando borramos filas en una tabla, también será muy importante usar la cláusula WHERE. Si no usas WHERE, borrarás todos los datos de la tabla.

```sql
DELETE FROM Clients
WHERE ID = 1;
```

## Consultas avanzadas de SQL (SUM, COUNT, AVG)

El siguiente paso es hacer operaciones con los datos. Lo que quieres no es únicamente consultar datos, los motores de bases de datos son mucho más potentes que eso. Primero crearás una tabla e insertarás datos como ya sabes hacer:

```sql
CREATE TABLE Sales (
    ID INT PRIMARY KEY,
    Product VARCHAR(255),
    Quantity INT,
    Price DECIMAL(10, 2)
);

INSERT INTO Sales (ID, Product, Quantity, Price)
VALUES
    (1, 'T-shirt', 100, 15.99),
    (2, 'Jeans', 50, 29.99),
    (3, 'Shoes', 30, 49.99),
    (4, 'Hat', 75, 9.99),
    (5, 'Socks', 200, 4.99);
```

Con estos datos, puedes calcular, por ejemplo, cuánto has ganado. Puedes usar `AS` para ponerle un alias a la operación de suma.

```sql
SELECT SUM(Price * Quantity) AS TotalEarn
FROM Sales;
```

También puedes contar cuántos productos has vendido.

```sql
SELECT COUNT(*) AS TotalSales
FROM Sales;
```

Y si utilizas la cláusula WHERE, puedes ver cuántos 'Jeans' has vendido. Con los datos proporcionados anteriormente, el resultado debería de ser 1.

```sql
SELECT COUNT(*) AS JeansSales
FROM Sales
WHERE Product = 'Jeans';
```

Y por último, puedes hacer la media del precio de los productos vendidos.

```sql
SELECT AVG(Price) AS AveragePrice
FROM Sales;
```

Existen muchas otras operaciones sencillas (MIN, MAX, GROUP BY, HAVING, etc.) que te facilitarán la vida con SQL. Pero al principio te he hablado de bases de datos relacionales, ¿qué significa relacionales?

## ¿Base de datos relacional?

SQL es relacional, porque todas las tablas de una base de datos pueden tener relaciones entre sí. Por ejemplo, has creado una tabla de `Sales` antes, pero lo habitual sería tener otra tabla de `Products`, porque si haces una venta, tiene que ser sobre un producto. Esto es lo que se conoce como una relación entre tablas.

Una vez sabes de la existencia de relaciones, tendrás que plantearte cómo hacer consultas JOIN, LEFT JOIN, etc. Pero esto te dejo que continues investigando.

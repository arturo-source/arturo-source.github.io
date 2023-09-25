---
title: "What is SQL?"
description: "Learn SQL in just a few minutes."
summary: "Learn SQL in just a few minutes."
date: 2023-09-12T18:26:15+02:00
draft: false
tags: ["tiny pills"]
---

SQL is a language used to query and manage relational databases.

SQL (Structured Query Language) was developed in the 1970s and has become a standard in the management and manipulation of relational databases. You will always use SQL with a database engine, which is a piece of software that contains the logic to manipulate data. It is very common to use a database engine in programming, because it allows us to not have to program the data logic from scratch.

## SQL Syntax

Each database engine (MySQL, MariaDB, PostgreSQL, SQLite, etc.) has additions to make data processing easier, but they all have the basic SQL syntax in common. Common operations you will want to do on a database are:

### Create a table

The first thing you'll want to do is create a table so you can insert data, delete it, etc.

```sql
CREATE TABLE Clients (
    ID INT PRIMARY KEY,
    FullName VARCHAR(50),
    Email VARCHAR(100)
);
```

This table will have:

- ID (unrepeatable identifier, that is why it is PRIMARY KEY, and it is an integer, that is why INT).
- FullName (text string, therefore VARCHAR, maximum length 50 characters).
- Email (text string, this time of length 100).

### Delete a table

This table can also be deleted.

```sql
DROP TABLE Clients;
```

### Query a table

```sql
SELECT * FROM Clients;
```

You can also choose which fields you want to get, `*` means **all**.

```sql
SELECT ID, FullName FROM Clients;
```

### Insert data into a table

```sql
INSERT INTO Clients (ID, FullName, Email)
VALUES (1, 'Barnes Smith', 'bsmith@mail.com');
```

### Update data from a table

The WHERE clause in an update is very important. If you don't use WHERE, all rows in the table will be updated.

```sql
UPDATE Clients
SET Email = 'newbsmith@mail.com'
WHERE ID = 1;
```

### Delete a row from a table

When we delete rows in a table, it will also be very important to use the WHERE clause. If you don't use WHERE, you will delete all the data in the table.

```sql
DELETE FROM Clients
WHERE ID = 1;
```

## Advanced SQL Queries (SUM, COUNT, AVG)

The next step is to do operations with the data. What you want is not just to query data, database engines are much more powerful than that. First you will create a table and insert data as you already know how to do:

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

With this data, you can calculate, for example, how much you have earned. You can use `AS` to alias the addition operation.

```sql
SELECT SUM(Price * Quantity) AS TotalEarn
FROM Sales;
```

You can also count how many products you have sold.

```sql
SELECT COUNT(*) AS TotalSales
FROM Sales;
```

And if you use the WHERE clause, you can see how many 'Jeans' you have sold. With the data provided above, the result should be 1.

```sql
SELECT COUNT(*) AS JeansSales
FROM Sales
WHERE Product = 'Jeans';
```

And finally, you can average the price of the products sold.

```sql
SELECT AVG(Price) AS AveragePrice
FROM Sales;
```

There are many other simple operations (MIN, MAX, GROUP BY, HAVING, etc.) that will make your life with SQL easier. But at the beginning I told you about relational databases, what does relational mean?

## Relational database?

SQL is relational, because all tables in a database can have relationships with each other. For example, you have created a `Sales` table before, but the usual thing would be to have another `Products` table, because if you make a sale, it has to be about a product. This is what is known as a relationship between tables.

Once you know the existence of relationships, you will have to consider how to make JOIN, LEFT JOIN queries, etc. But this I leave you to continue investigating.

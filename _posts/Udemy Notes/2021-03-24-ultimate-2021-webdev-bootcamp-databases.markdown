---
layout: post
title: Udemy - The Complete 2021 Web Development Bootcamp - Part 5 - Databases
categories: webdev
permalink: pretty
---

Course URL: (https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/12385614#overview)[https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/12385614#overview]

---

### Most popular node.js databases

- SQL/ Relational databases

  - SQL databases group data in tables; requires a structure.
  - Pros:
    - Familiar, basically like Excel tables
    - Good at establishing relationships
    - Faster to query
    - Good for things with lots of relationships together, e.g. store inventory
    - More mature
  - Cons:
    - Inflexible for data that doesn't fit in defined columns
    - Missing data is automatically set to "null"
    - Scales vertically. Less scalable, gets slower the more data you add and costly.
  - Examples:
    - MySQL
    - PostgreSQL

- NoSQL/ Nonrelational databases
  - NoSQL databases store data in document structure; more flexible to changes if schema not yet defined.
  - Pros:
    - More flexible
    - Good for one-to-many relationships, e.g. instagram storing data all pertaining to one person
    - Scales horizontally. More scalable, because records are all small JSON objects which can be distributed across many systems.
    - Shiny and new
  - Cons:
    - Less structured so not as good at establishing relationships
  - Examples
    - MongoDB
    - Redis

---

### SQL

#### Common Commands

```
-- Be sure to specify the field you want to be the primary key
-- It is best practice to set the primary key field as NOT NULL
CREATE TABLE table_name (
    column1 datatype NOT NULL,
    column2 datatype
    ...
    PRIMARY KEY (column1)
)

-- Use underscores for spaces with SQL
-- Foreign key joins a table with another table. REFERENCES indicates the table that the column corresponds with and the parentheses indicate which column in that table.
CREATE TABLE orders (
    id INT,
    order_number INT,
    customer_id INT,
    product_id INT,
    PRIMARY KEY (id),
    FOREIGN KEY (customer_id) REFERENCES customers(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
)

-- Inserts this row into products table, each parameter is a column value
INSERT INTO products
VALUES (1, "Pen", 1.20)

INSERT INTO orders
VALUES (1, 4362, 2, 1)

-- To skip a field, specify the columns you have data for
INSERT INTO products (id, name)
VALUES (2, "Pencil")

SELECT * FROM table WHERE criteria
SELECT * FROM products WHERE id=1

UPDATE table SET column1 = value1, column2 = value2, ... WHERE condition
-- Below will set ALL prices to 0.8!
UPDATE products SET price = 0.8

-- ALTER TABLE is used to add, delete, or modify columns in an existing table
ALTER TABLE table ADD column_name datatype;

DELETE FROM table WHERE condition

```

#### CRUD

- Create
- Read
- Update
- Destroy

#### SQL Notes

- Semicolons are not required

#### SQL Relationships, Foreign Keys, Inner Joins

```
-- INNER JOIN generic format
SELECT column_name(s)
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name

-- JOIN example

SELECT orders.order_number, customers.first_name, customers.last_name, customers.address
FROM orders
INNER JOIN customers ON orders.customer_id = customers.id

```

---

### MongoDB

#### Installing MongoDB

All of this is summarized with graphics at: (https://medium.com/@LondonAppBrewery/how-to-download-install-mongodb-on-windows-4ee4b3493514)[https://medium.com/@LondonAppBrewery/how-to-download-install-mongodb-on-windows-4ee4b3493514]

1. Go to MongoDB.com
2. Click on free community server. Download current release. (https://www.mongodb.com/try/download/community)[https://www.mongodb.com/try/download/community]
3. Follow the Quick Start Guide at the bottom of the page:
4. Move the unzipped downloaded folder to /usr/local/mongodb with `$ sudo mv [file address which can be populated by dragging the folder into terminal] /usr/local/mongodb `
5. Check that the folder was successfully moved with `$ open /usr/local/mongodb`
6. Add the mongo db binaries to environment variables by first cd to home `$ cd ~`. (`$ pwd ` should return Users/[your username])
7. `$ touch .bash_profile`
8. `$ open .`
9. ` vim .bash_profile`
   a. Get into insert mode by hitting "i"
   b. Type `export PATH-$PATH:/usr/local/mongodb/bin`
   c. Exit insert mode by hitting "esc"
   d. Save and quit vim with ":wq!"
10. Create the data directory. MongoDB by default writes all data to the /data/db directory. `$ mkdir -p /data/db`
    - Note this may not work on newer Macs where /data/db becomes read-only. You will have to create a separate /data/db folder. Follow the homebrew install instructions here: (https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)[https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/]

#### MongoDB CRUD Operations in the Shell

- Protip: Review CRUD first for every new database

1. `$ mongod` spins up the mongo server

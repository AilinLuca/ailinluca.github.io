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

For Macs, follow the homebrew install instructions here: (https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)[https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/]

- Protip: Review CRUD first for every new database

#### Create

1. For macs, `$ mongod --config /usr/local/etc/mongod.conf --fork` spins up the mongo server and runs it as a background process
   a. Confirm this worked with `$ ps aux | grep -v grep | grep mongod`
2. `$ mongo` starts the mongo shell
3. See your 3 default databases with `> show dbs`. admin, config, and local.
4. `> use shopDB`
5. `> db` tells you which db you are working in.
6. Create db with `> db.collection.insertOne()` e.g. `> db. products.insertOne({_id: 1, name: "Pen", price: 1.20})`
   - If products doesn't exist, this code will create it in shopDB
   - Collections are mongoDB's version of tables.
7. In this case, `> show collections` will show product

#### Read

8. `> db.collection.find(query, projected)` queries data (query is the filter, project is the field to return). If you do not specify parameters, all data will display.
   a. `> db.products.find({name: "Pencil"})` finds only products where the name is "Pencil"
   b.`> db.product.find({price: {$gt: 1.0}})` finds only products where the price is greater than 1.0
   c. `> db.users.find({age: {$gt: 18}}, {name: 1, address: 1}).limit(5)` finds the users older than 18 and returns their name and address. The "1" in the projection specifies that return name and address are true.

#### Update

9. `> db.products.updateOne({_id: 1}, {$set: {stock: 32}})` will query for item with id = 1 and create a stock field with value 32.

#### Delete

10. `> db.products.deleteOne({_id: 2})` will delete the ite with id = 2.

#### Relationships in MongoDB

- You can embed documents within other documents. E.g. you can add a reviews document in the pencil product:

```> db.product.updateOne({name: "Pencil"}, {$set: {review: [{author: "Jane Doe", rating: 5, review: "Best pencil ever!"}]}})

```

- You can have an array that references the ids of other items. e.g. here productsOrdered is an array of product ids.

```
{
    _id: 1,
    name: "Pen",
    price: 1.20,
    stock: 32
}

{
    _id: 2,
    name: "Pencil",
    price: 0.80,
    stock: 12
}

{
    orderNumber: 3243,
    productsOrdered: [1, 2]
}

```

#### MongoDB with Node.js

Two options:

1. Use Native Driver.
2. Use ODM called Mongoose
   a. Mongoose is more popular. It cuts down on code.

MongoDB Native Driver

- See the drivers at (https://docs.mongodb.com/ecosystem/drivers/)[https://docs.mongodb.com/ecosystem/drivers/]. There is a Node.js driver guide.
- Create a new project following the quickstart guide here: (https://mongodb.github.io/node-mongodb-native/3.5/quick-start/quick-start/)[https://mongodb.github.io/node-mongodb-native/3.5/quick-start/quick-start/]

```
// Sample fruits project db app.js

const MongoClient = require("mongodb").MongoClient;
const assert = require("assert");

// Connection URL
const url = "mongodb://localhost:27017";

// Database Name
const dbName = "fruitsDB";

// Create a new MongoClient
const client = new MongoClient(url);

// Use connect method to connect to the Server
client.connect(function (err) {
  assert.strictEqual(null, err);
  console.log("Connected successfully to server");

  const db = client.db(dbName);

  //   insertDocuments(db, function () {
  //     client.close();
  //   });
  findDocuments(db, function () {
    client.close();
  });
});

const insertDocuments = function (db, callback) {
  // Get the document collection
  const collection = db.collection("fruits");
  // Insert some documents
  collection.insertMany(
    [
      {
        name: "Apple",
        score: 8,
        review: "Great fruit",
      },
      {
        name: "Orange",
        score: 6,
        review: "Kind of sour",
      },
      {
        name: "Banana",
        score: 9,
        review: "Great stuff!",
      },
    ],
    function (err, result) {
      assert.strictEqual(err, null);
      assert.strictEqual(3, result.result.n);
      assert.strictEqual(3, result.ops.length);
      console.log("Inserted 3 documents into the collection");
      callback(result);
    }
  );
};

const findDocuments = function (db, callback) {
  // Get the documents collection
  const collection = db.collection("fruits");
  // Find some documents
  collection.find({}).toArray(function (err, fruits) {
    assert.strictEqual(err, null);
    console.log("Found the following records");
    console.log(fruits);
    callback(fruits);
  });
};
```

---

### Mongoose

- Docs here: (https://mongoosejs.com/docs/)[https://mongoosejs.com/docs/]

1. Cd into your project.
2. `$ npm i mongoose`

#### Deleting databases

- Switch to the database with `> use fruitsDB`
- `> db.dropDatabase()`

```
// mongoose app.js sample

const mongoose = require("mongoose");
// Opens mongo connection
mongoose.connect("mongodb://localhost:27017/fruitsDB", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
}); // this one line will create the database fruitsDB if it doesn't already exist

const fruitSchema = new mongoose.Schema({
  name: {
    type: String,
    // This will make name required
    required: [true, "Name is required"],
  },
  rating: {
    type: Number,
    // You can add built-in validators such as min and max
    // Data that doesn't match the validation will not be inserted
    min: 1,
    max: 10,
  },
  review: String,
});

const Fruit = mongoose.model("Fruit", fruitSchema); // here establish the collection with the singular item of the collection Fruits and associated schema

const apple = new Fruit({
  name: "Apple",
  rating: 7,
  review: "Pretty solid as a fruit.",
});

//fruit.save(); // will save fruit document into fruit collection in fruitsDB
// comment out if you don't want to keep adding the same fruit to the collection each time you run app.js

// You can also add a collection of people to your fruitsDB
const personSchema = new mongoose.Schema({
  name: String,
  age: Number,
  favoriteFruit: fruitSchema, // This is how you create a relationship
});

const Person = mongoose.model("Person", personSchema);

const person = new Person({
  name: "John",
  age: 37,
  favoriteFruit: apple,
});

// person.save();

// You can also add multiple fruit at once
const kiwi = new Fruit({
  name: "Kiwi",
  score: 10,
  review: "The best fruit!",
});

const orange = new Fruit({
  name: "Orange",
  score: 4,
  review: "Too sour",
});

const banana = new Fruit({
  name: "Banana",
  score: 10,
  review: "So happy",
});

// Fruit.insertMany([kiwi, orange, banana], function (err) {
//   if (err) {
//     console.log(err);
//   } else {
//     console.log("Successfully saved all the fruits to fruitsDb");
//   }
// });

// To find all the documents in your collection
Fruit.find(function (err, fruits) {
  if (err) {
    console.log(err);
  } else {
    // It's good practice to close mongo connection when you are done
    // This way you don't have to Ctrl+C out of terminal
    // mongoose.connection.close();
    fruits.forEach((a) => console.log(a.name));
  }
});

// How to update data with mongoose
Fruit.updateOne(
  { _id: "605e7f39c210bbb6d9900818" },
  { name: "Kiwano" },
  function (err) {
    if (err) {
      console.log(err);
    } else {
      console.log("Successfully updated the documents");
    }
  }
);

// How to delete data with mongoose
Fruit.deleteOne({ name: "Orange" }, function (err) {
  if (err) {
    console.log(err);
  } else {
    console.log("Successfully deleted document");
  }
});

```

---

### Updating todo list app with a database

#### App.js

```
const express = require("express");
const app = express();
const mongoose = require("mongoose");
const _ = require("lodash");

const date = require(__dirname + "/date.js");

const port = 3000;

app.use(express.static("public"));
app.use(express.urlencoded({ extended: true }));

mongoose.connect("mongodb://localhost:27017/todolistDB", {
  useNewUrlParser: true,
});

const itemsSchema = {
  item: String,
};

const Item = mongoose.model("Item", itemsSchema);

const item1 = new Item({
  item: "Add your todos in the input below",
});

const item2 = new Item({
  item: "<-- check box to delete",
});

const defaultItems = [item1, item2];
const day = date.getDate();

app.set("view engine", "ejs");

app.get("/", (req, res) => {
  Item.find({}, (err, items) => {
    if (err) {
      console.log(err);
    } else {
      if (items.length === 0) {
        Item.insertMany(defaultItems, (err) => {
          if (err) {
            console.log(err);
          } else {
            console.log("Successfully inserted default items");
          }
        });
      }
      res.render("list", { listTitle: day, newListItems: items });
    }
  });
});

app.post("/", (req, res) => {
  const newItem = req.body.newItem;
  const listName = req.body.list;

  const item = new Item({
    item: newItem,
  });

  if (listName === day) {
    item.save();
    res.redirect("/");
  } else {
    List.findOne({ name: listName }, (err, foundList) => {
      if (err) {
        console.log(err);
      } else {
        foundList.items.push(item);
        foundList.save();
        res.redirect("/" + listName);
      }
    });
  }
});

app.post("/delete", (req, res) => {
  const checkedItemId = req.body.checkbox;
  const listName = req.body.listName;

  console.log("checkedItemId: " + checkedItemId);
  console.log("listName: " + listName);

  if (listName === day) {
    Item.findByIdAndRemove(checkedItemId, function (err) {
      // note callback required to delete otherwise will only return query
      if (err) {
        console.log(err);
      } else {
        console.log("Successfully deleted item");
        res.redirect("/"); // needed to reflect change
      }
    });
  } else {
    List.findOneAndUpdate(
      { name: listName },
      { $pull: { items: { _id: checkedItemId } } },
      (err) => {
        if (err) {
          console.log(err);
        } else {
          res.redirect("/" + listName);
        }
      }
    );
  }
});

const listSchema = {
  name: String,
  items: [itemsSchema],
};

const List = mongoose.model("List", listSchema);

// To dynamically render custom lists
app.get("/:listName", (req, res) => {
  const listName = _.capitalize(req.params.listName);
  List.findOne({ name: listName }, (err, foundList) => {
    if (err) {
      console.log(err);
    } else if (foundList) {
      console.log("List already exists");
      res.render("list", {
        listTitle: foundList.name,
        newListItems: foundList.items,
      });
    } else if (!foundList) {
      console.log("Doesn't exist");
      const list = new List({
        name: listName,
        items: defaultItems,
      });
      list.save();
      res.redirect("/" + listName);
    }
  });
});

app.get("/about", (req, res) => {
  res.render("about");
});

app.listen(port, () => {
  console.log(`Server started on port ${port}`);
});

```

#### list.ejs

```
<%- include("header"); -%>
<h1><%= listTitle %></h1>
<form action="/delete" method="POST">
    <% newListItems.forEach(a => { %>
      <div></div><label><input type="checkbox" name="checkbox" value="<%= a._id %>" onchange="this.form.submit()"> <%= a.item %></label></div>
      <% }) %>
    <input type="hidden" name="listName" value="<%= listTitle %>"></input>
</form>

<form action="/" method="POST">
  <input type="text" name="newItem" />
  <button type="submit" name="list" value=<%=listTitle%>>Add</button>
</form>
<%- include("footer"); -%>

```

---

### Deploying web apps with databases

- node.js app will be hosted on heroku
- mongodb database will be hosted on mongodb atlas

#### Set up MongoDB Atlas

It is well documented. Use the free AWS hosting based in North Virginia to create a cluster and complete the checklist.

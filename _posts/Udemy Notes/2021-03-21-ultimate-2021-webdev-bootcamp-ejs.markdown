---
layout: post
title: Udemy - The Complete 2021 Web Development Bootcamp - Part 4 - EJS
categories: webdev
permalink: pretty
---

Course URL: [https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/12384810#overview](https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/12384810#overview)

---

```
// Fun snippet

app.get("/", (req, res) => {
var today = new Date();
if (today.getDay() === 6 || today.getDay() === 0) {
    // saturday or sunday
    // sunday through saturday: 0 - 6
    res.send("Yay it's the weekend!");
} else {
    res.send("Boo! I have to work!");
}
});

```

### Why do we need templates?

- If you want to send multi-line HTML, res.write() repeatedly is annoying to write.
- Sending over separate web pags is inefficient too, want to minimize repetitive code; create HTML template to change certain parts of the code only.

### EJS

- Documentation [ejs.co](ejs.co)

#### Getting started

1. `$ npm i ejs`
2. Add to app.js `app.set('view engine', 'ejs');`
3. In order to use ejs (and most other view engines) create a new folder called 'views'. `$ mkdir views`
4. Inside 'views' create base template `$ touch list.ejs`. Copy all HTML template into this file.
   - EJS marker is formatted like this `<%= EJS %>`

```
<!-- list.ejs -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>It's a <%= kindOfDay %>!</h1>
  </body>
</html>

```

```
// app.js
const express = require("express");
const port = 3000;
const app = express();

app.set("view engine", "ejs");

app.get("/", (req, res) => {
  var todayDay = new Date().getDate();
  if (todayDay === 6 || todayDay === 0) {
    // saturday or sunday
    day = "Weekend";
  } else {
    day = "Weekday";
  }
  res.render("list", { kindOfDay: day }); //note most people would name as {day: day}
});

app.listen(port, () => {
  console.log(`Server started on port ${port}`);
});

```

#### EJS tags

- (ejs.co/#docs)[ejs.co/#docs]
- Different tags trigger different outputs
  - `<%= variable tag =>`
    - Outputs value into the template
  - `<% scriptlet tag %>`
    - For control flow, no output
    - Demo:
    ```
    <% if (day === "Monday") {%>
        <h1 style="color: purple"><%= day %> ToDo List</h1>
    <% else {%>
        <h1 style="color: blue"><%= day %> ToDo List</h1>
    <% } %>
    ```
    - You do not want to write complex javascript with these; that should be handled by server.
    - This is only for control flow and things that would be hard for the server.

#### Passing data from webpage to server

```
// Updated date function

app.get("/", (req, res) => {
  var today = new Date();
  var options = {
    weekday: "long",
    day: "numeric",
    month: "long",
  };
  var day = today.toLocaleDateString("en-US", options);

  res.render("list", { day: day });
    });

    app.listen(port, () => {
    console.log(`Server started on port ${port}`);
});
```

- Note that with each res.render() the page re-renders, so it is expecting all the variables to be defined, or else the app will crash.

---

#### Javascript scope

- Javascript has strict functional scope
- Variables declared in non-function blocks of code -- for loops, while loops, if-else statements -- are accessible outside of them.
- Variable keywords
  - All 3 are local variables when created inside a function and global when created outside
  - var
    - Global when declared within non-function blocks
    - Unlike many other languages where this is local
  - let
    - Local when declared within non-function blocks
    - Preferred over var
  - const
    - Variable cannot be changed after declaration
    - Note that this does not protect items inside this object -- you can and should declare arrays with const, which will stop the variable from being reassigned, but will still allow you to add and change items in the array.

---

#### Adding CSS to Express apps

1. Create a folder called "public"
2. Inside it, create a folder called "css"
3. Put styles.css in "css"
4. in app.js add `app.use(express.static("public"));`
5. Now you can link stylesheet in .ejs files in the head as usual

- `input[type="checkbox"] {}` will target only checkboxes
- `input:checked+p {}` will target all the p elements after the checked inputs

---

### Templating

#### You can reuse a single ejs page by adding a control check for req.body identifier "list" attached to the submit button

- name of button is set to "list"
- value of button is set dynamically to listName
- depending on the value, app.get("work") or app.get("/") render data will fire

```
//app.js

const express = require("express");
const port = 3000;
const app = express();
app.use(express.urlencoded({ extended: true }));

let items = [];
let workItems = [];

app.set("view engine", "ejs");

app.get("/", (req, res) => {
  let today = new Date();
  let options = {
    weekday: "long",
    day: "numeric",
    month: "long",
  };
  let day = today.toLocaleDateString("en-US", options);

  res.render("list", { listTitle: day, newListItems: items });
});

app.post("/", (req, res) => {
  let item = req.body.newItem;

  if (req.body.list === "Work") {
    workItems.push(item);
    res.redirect("/work");
  } else {
    items.push(item);
    // res.render("list", {newListItem: items}); This will not work because all variables must be defined at render
    res.redirect("/");
  }
});

app.get("/work", (req, res) => {
  res.render("list", { listTitle: "Work List", newListItems: workItems });
});

app.listen(port, () => {
  console.log(`Server started on port ${port}`);
});
```

```
<!-- list.ejs -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1><%= listTitle %>!</h1>
    <ul>
      <% newListItems.forEach(a => { %>
      <li><%= a %></li>
      <% }) %>
    </ul>
    <form action="/" method="POST">
      <input type="text" name="newItem" />
      <button type="submit" name="list" value=<%=listTitle %> >Add</button>
    </form>
  </body>
</html>
```

#### Layouts can be used for repeating page components, like headers and footers

- layouts are just ejs files with HTML snippets in the views folder
- include them on other ejs pages with `<%- include("header"); -%>`

---

### Node modules

- We want the app.js file to only include routing information, so we move the date calculation function to a new file
- In app.js `require(__dirname + "/date.js");`
- In date.js, export the function with `module.exports = getDate;` at the top of the file
  - Note `exports` is the more commonly-used abbreviation of `module.exports`

```
// full date.js file

module.exports.getDate = getDate; // you can export more than one function using dot notation because module.exports is a js object
module.exports.getDay = getDay;

function getDate() {
  const today = new Date();

  const options = {
    weekday: "long",
    day: "numeric",
    month: "long"
  };

  let day = today.toLocaleDateString("en-US", options);
  return day;
}

function getDay() {
  const today = new Date();
  const options = {
    weekday: "long"
  };
  let day = today.toLocaleString("en-US", options);
  return day;
}
```

- In app.js, run the function with `let day = date.getDate();`

#### const in Javascript

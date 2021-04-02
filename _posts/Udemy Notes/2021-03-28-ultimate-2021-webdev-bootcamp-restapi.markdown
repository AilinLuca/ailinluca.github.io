---
layout: post
title: Udemy - The Complete 2021 Web Development Bootcamp - Part 6 - REST APIs
categories: webdev
permalink: pretty
---

Course URL: (https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/12385614#overview)[https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/12385614#overview]

---

### What is REST?

- The API is the menu of things the client can order from the server
- REpresentational State Transfer
  - A style for designing APIs developed by Roy Fielding in 2000
  - Other styles:
    - SOAP
    - GraphQL
    - Falcor

#### What makes a request RESTful?

- Uses HTTP Request verbs

  - GET
    - Read
  - POST
    - Create
  - PUT
    - Update
    - Entire new entry
  - PATCH
    - Update
    - Update only a piece of the entry that needs to be updated
  - DELETE
    - Delete

- Use specific pattern of routes/endpoint URLs
  - GET /articles gets all articles, GET /articles/jack-bauer gets only the article on Jack Bauer
  - POST /articles creates a new article
  - PUT or PATCH /articles/jack-bauer updates the article on Jack Bauer
  - DELETE /articles deletes all articles while DELETE /articles/jack-bauer deletes the article on Jack Baeur

---

### Creating a database with Robo 3T

- Robo 3T is a GUI for working with mongoDB databases. It makes it easier to do things that you can also do in the command line.

1. Go to robomongo.org and download Robo 3T
2. Add data to new database
3. Set up an app with npm, express, ejs, and mongodb
4. App.js code below

```
const ejs = require("ejs");
const { request } = require("express");
const express = require("express");
const mongoose = require("mongoose");
const _ = require("lodash");
const app = express();

app.set("view engine", "ejs");
app.use(express.urlencoded({ extended: true }));
app.use(express.static("public"));

mongoose.connect("mongodb://localhost:27017/wikiDB", {
  useNewUrlParser: true,
});

const articleSchema = {
  title: String,
  content: String,
};

const Article = mongoose.model("Article", articleSchema);

// app.get("/articles", (req, res) => {
//   Article.find((err, articles) => {
//     if (err) {
//       res.send(err);
//     } else if (!err) {
//       res.send(articles);
//     }
//   });
// });

// app.post("/articles", (req, res) => {
//   //   If you make a postman request to localhost:3000/articles, the data will log to the console using the code below
//   //   console.log(req.body.title);
//   //   console.log(req.body.content);

//   const article = new Article({
//     title: req.body.title,
//     content: req.body.content,
//   });

//   // Be sure to send something, so your postman requests will receive the expected response
//   article.save((err) => {
//     if (err) {
//       res.send(err);
//     } else if (!err) {
//       res.send("Successfully added a new article.");
//     }
//   });
// });

// app.delete("/articles", (req, res) => {
//   Article.deleteMany((err) => {
//     if (err) {
//       res.send(err);
//     } else if (!err) {
//       res.send("All articles successfully deleted");
//     }
//   });
// });

/* New and improve app.routes() version of the above */
app
  .route("/articles")
  .get((req, res) => {
    Article.find((err, articles) => {
      if (err) {
        res.send(err);
      } else if (!err) {
        res.send(articles);
      }
    });
  })
  .post((req, res) => {
    //   If you make a postman request to localhost:3000/articles, the data will log to the console using the code below
    //   console.log(req.body.title);
    //   console.log(req.body.content);

    const article = new Article({
      title: req.body.title,
      content: req.body.content,
    });

    // Be sure to send something, so your postman requests will receive the expected response
    article.save((err) => {
      if (err) {
        res.send(err);
      } else if (!err) {
        res.send("Successfully added a new article.");
      }
    });
  })
  .delete((req, res) => {
    Article.deleteMany((err) => {
      if (err) {
        res.send(err);
      } else if (!err) {
        res.send("All articles successfully deleted");
      }
    });
  });

const port = 3000;
app.listen(port, function () {
  console.log("Server started on port " + port);
});

app
  .route("/articles/:articleTitle")
  .get((req, res) => {
    {
      Article.findOne({ title: req.params.articleTitle }, (err, article) => {
        if (err) {
          res.send(err);
        } else if (!article) {
          res.send("No article of that title was found.");
        } else if (article) {
          res.send(article);
        }
      });
    }
  })
  .put((req, res) => {
    Article.updateOne(
      { title: req.params.articleTitle },
      {
        title: req.body.title,
        content: req.body.content,
      },
      // { overwrite: true }, // mongoose by default has overwrite: false, while mongoDB has overwrite: true
      // however now that update() is deprecated, this option is made redundant in updateOne
      (err, results) => {
        if (err) {
          res.send(err);
        } else if (!err) {
          res.send("Successfully updated article");
        }
      }
    );
  })
  .patch((req, res) => {
    Article.findOneAndUpdate(
      { title: req.params.articleTitle },
      { $set: req.body },
      (err, results) => {
        if (err) {
          res.send(err);
        } else if (!err) {
          res.send("Successfully updated article");
        }
      }
    );
  })

  .delete((req, res) => {
    Article.deleteOne({ title: req.params.articleTitle }, (err) => {
      if (err) {
        res.send(err);
      } else if (!err) {
        res.send(
          `Article titled "${req.params.articleTitle}" successfully deleted`
        );
      }
    });
  });

```

5. You can make a CRUD requests to this API with Postman
   - Set the url to `localhost:3000/articles`
6. In Express, you can use app.route() to create chainable event handlers to reduce redundancy and typos.

```
app.route('/book')
    .get((req, res) => {
        res.send('Get a random book')
    })
    .post((req, res) => {
        res.send('Add a book')
    })
    .put((req, res) => {
        res.send('Update the book')
    })
```

7. When searching for articles, remember that spaces are represented in URLs as %20 e.g. `/articles/jack%20bauer`
8. When you PUT and only add one property, the other missing properties will be set to null.
9. If you don't want to replace entire document, then PATCH.

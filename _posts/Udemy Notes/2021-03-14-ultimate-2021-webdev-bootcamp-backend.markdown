---
layout: post
title: Udemy - The Complete 2021 Web Development Bootcamp - Part 3 - Backend Review
categories: webdev
permalink: pretty
---

Course URL: [https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/16842810#overview](https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/16842810#overview)

---

### Command line shortcuts

- Editing commands

  - Ctrl + A
  - To the front of the command
  - Ctrl + E
  - To the end of the command
  - Ctrl + U
  - Clears the current command
  - Option + click
  - Select where you want to edit the command
  - Left and right arrows
  - Navigate through the command

- Navigating

  - cd ~
    - To home
  - cd De + tab
    - Autocomplete
    - 1 tab to autocomplete; if that doesn't work 2 will give you list of possibilities
  - To open folder in VSCode, cd into folder and then type `$ code -a .`

- Files

  - touch
  - open -a VSCode code.js
  - rm code.js
    - rm \*
    - To remove files
  - rm -r
    - To remove directories
    - rm -rf --no-preserve-root
      - To wipe your hard disk

---

### Node.js

- Allows us to use JS to interact with the hardware of the computer and program desktop applications, and on servers to process user requests, which expands the capability of websites.

#### To install Node.js

- Mac: [https://nodejs.org/en/](https://nodejs.org/en/)
- Windows: [https://nodejs.org/en/](https://nodejs.org/en/)
- Confirm in terminal with `$ node --version`

#### Using node

- `$ node index.js`
  - Runs the js file with Node

#### REPL

- Read Evaluation Print Loop
  - Execute code in bite-size chunks
  - Installing Node installs the Node REPL
  - Activate by typing `$ node` in the terminal
  - Works like the Chrome console
  - To exit the REPL
    - `.exit` or Ctrl + C twice

#### Native Node modules

- Node comes bundled with modules automatically
  - (https://nodejs.org/api/)[https://nodejs.org/api/]
- e.g. File System module helps you interact with the local file system
  1. Require file system in index.js
     - `const fs = require("fs");`
  2. Now you can do the things in the api: (https://nodejs.org/api/fs.html)[https://nodejs.org/api/fs.html]
     - For example the following line of code in index.js, when run
  ```
  fs.copyFileSync("file1.txt", "file2.txt");
  // Looks in current directory and will copy file1.txt to file2.txt
  ```

#### External node modules

- Node Package Manager (NPM)
  - `npm init` creates package.json which describes all npm packages in the project
    - Set up: package name, version, description, entry point (index.js), test command, git repository, keywords, author, license
  - `npm install name-of-module`
    - Module will be added as a dependency
  - ` var superheroes = require("name-of-module");` in index.js
  - Then you can use the methods from the module

---

### Express.js

- Express is a node framework to help you write less repetitive code
- Documentation: [https://expressjs.com](https://expressjs.com)

#### How to create a server with Node.js and Express.js

- `npm install express`
- Create a file called server.js and then run node server.js to start the Express server.
  - (Nodemon)[https://www.npmjs.com/package/nodemon] is a utility that will monitor for changes in source and automatically restart the server
    - Install with `$ npm install -g nodemon`
    - Run with `$ nodemon server.js`
    - There is a common bug

#### Express syntax

- Express serves up data based on route specified

  ```
  const express = require("express");
  const app = express();
  const port = 3000;

  // home route is "/"
  // app.get handles get requests to home route
  app.get("/", (req, res) => {
  res.send("Hello World!");
  });

  app.get("/contact", (req, res) => {
      res.send("Contact me at ailin@email.com");
  });

  // sendFile sends a file
  // __dirname gets the path of the current file no matter whose computer you're on or where you are
  app.get("/form", (req, res) => {
      res.sendFile(__dirname + "/index.html");
  });

  // app.post handles post requests to home route
  app.post("/", (req, res) => {
    res.send("Thanks for posting that!");
  });

  app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`);
  });

  ```

- For Express 4.16+, body-parser is deprecated and functionality is re-incorporated back into Express. Non-breaking change.
  - Use `app.use(express.urlencoded());` and `app.use(express.json());` instead of `app.use(bodyParser.urlencoded({extended: true}));` and `app.use(bodyParser.json());`

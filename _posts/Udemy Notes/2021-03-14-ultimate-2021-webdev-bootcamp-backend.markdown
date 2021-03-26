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
  // res.send() sends the response. You can only have one in any given app method.
  // To send multiple lines, use res.write() and then res.send()
  app.get("/", (req, res) => {
      res.write("Hello");
      res.write("World");
      res.send();
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

  app.use(express.urlencoded({extended: true})); // parses post data
  app.post("/", (req, res) => {
      var n1 = req.body.n1;
      var n2 = req.body.n2;
      var sum = n1 + n2;
      res.send("Your sum is " + sum);
  })

  app.listen(port, () => {
    console.log(`Example app listening at http://localhost:${port}`);
  });

  ```

- For Express 4.16+, body-parser is deprecated and functionality is re-incorporated back into Express. Non-breaking change.
  - Use `app.use(express.urlencoded());` and `app.use(express.json());` instead of `app.use(bodyParser.urlencoded({extended: true}));` and `app.use(bodyParser.json());`

---

### APIs

- Application Programming Interfaces
  - A set of commands, functions, protocols, and objects that programmers can use to create software or interact with an external system.
  - A contract between data provider and developer.

#### API structure

- There is an endpoint url
- Then after it are the queries
  - If multiple queries, followed by a question mark and joined by ampersands. e.g. `?blacklistFlags=nsfw&type=single&contains=debugging`
  - Order of the queries does not matter

#### How to make calls inside node.js

From (https://nodejs.org/api/https.html)[https://nodejs.org/api/https.html]

```
const https = require('https');

// Note how url has https://. This is required.
https.get('https://encrypted.google.com/', (res) => {
  console.log('statusCode:', res.statusCode);
  console.log('headers:', res.headers);

  res.on('data', (d) => {
    process.stdout.write(d);
  });

}).on('error', (e) => {
  console.error(e);
});
```

- Fun: (httpstatusdogs.com)[httpstatusdogs.com]

#### Demo weather request

```
// WIP notes

const https = require("https");

app.get("/", (req, res) => {
  const apiKey = "";
  const url = `https://api.openweathermap.org/data/2.5/weather?q=London&appid=${apiKey}`;

  https.get(url, (response) => {
    console.log(response);
    console.log(`status code: ${response.statusCode}`);
    response.on("data", (data) => {
      console.log(data); // prints data in hexadecimal code
      const weatherData = JSON.parse(); // returns JSON in a js object
      console.log(weatherData);
      console.log(weatherData.main.temp);
    });
  });
});
```

- JSON.parse() turns JSON into a javascript object
- JSON. stringify() turns a javascript object into a string
- JSON viewer browser extension will let you quickly copy the JSON path

```
// Final app.js
const express = require("express");
const app = express();
const port = 3000;

const https = require("https");

app.use(express.urlencoded());

app.get("/", (req, res) => {
  res.sendFile(__dirname + "/index.html");
});

app.post("/", (req, res) => {
  const apiKey = "";
  const query = req.body.cityName;
  const unit = "imperial";
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${query}&appid=${apiKey}&units=${unit}`;

  https.get(url, (response) => {
    console.log(`status code: ${response.statusCode}`);
    response.on("data", (data) => {
      const weatherData = JSON.parse(data); // returns JSON in a js object
      const temp = weatherData.main.temp;
      const weatherDescription = weatherData.weather[0].description;
      const icon = weatherData.weather[0].icon;
      const imageURL = `http://openweathermap.org/img/wn/${icon}@2x.png`;
      res.write(`<p>The weather is currently ${weatherDescription}</p>`);
      res.write(
        `<h1>The temperature in ${query} is ${temp} degrees Fahrenheit.</h1>`
      );
      res.write(`<img src="${imageURL}" />`);
      res.send();
    });
  });
});

app.listen(port, () => {
  console.log("server is running on port " + port);
});

```

```
<!-- Final index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <form action="/" method="post">
      <label for="cityInput">City Name</label>
      <input id="cityInput" type="text" name="cityName" placeholder="City" />
      <button type="submit">Go</button>
    </form>
  </body>
</html>

```

---

#### Sign-up Page Project

```
// Final app.js
const { response } = require("express");
const express = require("express");
const https = require("https");
const request = require("request");

const app = express();
const port = 3000;

// express.static is needed to serve up static files like local css and images which we have saved in public folder
// file references are written as if you are inside the public folder
app.use(express.static("public"));
app.use(express.urlencoded());

app.get("/", (req, res) => {
  res.sendFile(__dirname + "/signup.html");
});

app.post("/", (req, res) => {
  const apiKey = "";
  const listID = "";
  const url = `https://us1.api.mailchimp.com/3.0/lists/${listID}`;
  const options = {
    method: "POST",
    auth: `Ailin:${apiKey}`,
  };
  // remember these values are set by the name attribute
  // and the form must be set to action="/" and method="POST"
  const firstName = req.body.firstName;
  const lastName = req.body.lastName;
  const email = req.body.email;

  const data = {
    members: [
      {
        email_address: email,
        status: "subscribed",
        merge_fields: {
          FNAME: firstName,
          LNAME: lastName,
        },
      },
    ],
  };

  const jsonData = JSON.stringify(data);
  const request = https.request(url, options, (response) => {
    response.on("data", (data) => {
      console.log(JSON.parse(data));
    });
  });

  request.write(jsonData);
  request.end();

  if (response.statusCode === 200) {
    res.sendFile(__dirname + "/success.html");
  } else {
    res.sendFile(__dirname + "/failure.html");
  }
});

app.post("/failure", (req, res) => {
  res.redirect("/");
});

app.listen(port, () => {
  console.log(`Server is running and listening at port ${port}`);
});

```

---

### Deploying your server with Heroku

- github pages only hosts static sites

Getting started with Heroku:

- Create heroku account
- [Install heroku CLI](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up)
- heroku login
- prepare the app
  - Replace local port with process.env.PORT || 3000, a dynamic port that Heroku will define on the go or 3000 if running locally
  - Define a Procfile, which is a file in root with no type extension that contains instructions on how to start the app:
    - `web: node app.js`
- save work to git
- Create a new app on Heroku with `heroku create`. This creates a server and returns server links.
- `git push heroku master` will push local version to heroku
- `heroku logs` will show you crash logs
- Henceforth, you can update your app with `git add`, `git commit`, and `git push heroku master`

---

### Git and Github Review

#### Git commands

```
git add .
    // * is not part of git - it's a wildcard interpreted by the shell. * expands to all the files in the current directory, and is only then passed to git, which adds them all.

   // . is the current directory itself, and git adding it will add it and the all the files under it.

    // git add . and git add -A have no exceptions
    // git add * excludes files from current directory whose name begins with a dot
    // git add -u excludes new files

git status
    // displays the state of the working directory and the staging area. It lets you see which changes have been staged, which haven't, and which files aren't being tracked by Git

git commit -m "Present tense summary of changes made"

git log
    // shows what commits were made with unique commit IDs
    // HEAD shows you current status
    // type 'q' or 'z' to escape

git diff
    // shows you the difference between specified fiels

git checkout file.name
    // will rollback to the last committed version in local repo

git remote add origin https://github.com/reponame
    // Add the remote repo

git push -u origin main
    // pushes local repo with the -u option which links remote and local repos

git rm --cached -r .
    // removes all files in the staging area

git branch new-branch-name
    // add branch

git branch
    // shows you all your branches including the branch you are on

git checkout other-branch
    // switches to other-branch

git checkout main
git merge other-branch
:q!
    // Will merge other-branch info into main

git push -u origin main
    // Push the changse to remote repo

```

#### Git directories

- Working Directory is what git tracks
- Staging Area is where added files go
- Local Repository is where commits go
- Remote Repository is Github

#### Git ignore file

- Specify on each newline a filename that you want git to ignore
- Common gitignores are
  - . DS_Store
- Will accept wildcards like \*
- Use # to comment
- See github's premade gitignore file templates
  : (https://github.com/github/gitignore)[https://github.com/github/gitignore]

#### More git practice: (https://learngitbranching.js.org/)[https://learngitbranching.js.org/]

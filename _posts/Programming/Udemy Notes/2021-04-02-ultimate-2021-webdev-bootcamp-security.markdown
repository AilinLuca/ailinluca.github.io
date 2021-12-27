---
layout: post
title: The Complete 2021 Web Development Bootcamp - Part 7 - Authentication & Security
categories: udemy webdev
permalink: /:title
---

Course URL: (https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/13559376#notes)[https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/13559376#notes]

---

### Authentication intro

- Authenticate to:

  - Correlate created data with individuals
  - Restrict access to certain parts of the website

- Protip:
  If you would like to see the code after a certain commit, for example, you can simply type: `git checkout 7078af837299a4ff50121d67afe17d9fa522ec68 .` where the string of numbers is the commit ID.

  - The period in this command is importan

- See examples of these encryption methods on (https://cryptii.com)[https://cryptii.com]

---

#### Level 1 Security: Username and Password

- Store username and password to database
- Not sufficient! Username and password should be encrypted in the database.

#### Level 2 Security: Encryption

- Caesar cipher

  - Letter substitution cipher. Key is the number of letters to shift the alphabet by.

- mongoose-ecryption NPM package
  - Uses AES-256-CBC
  - Can encrypt and authenticate
  - Uses a plugin to mongoose schema
  - Automatically encrypts on save and decrypts on find
  - By default encrypts entire database; specify encryption fields by passing the parameter `encryptedFields: ["password"] `
  - If a hacker finds the secret, security will be compromised

#### Level 3 Security: Using Environment Variables to Keep Secrets Safe

- dotenv NPM package

  1. `npm i dotenv`
  2. Add `require("dotenv").config();` to the top of app.js. Must be near top so it can be used by anything on the page.
  3. Create a .env file at root of the folder.
  4. Create the environments in .env

  - Must be formatted `ENV=definition` without any spaces or quotes
  - Must be snake-case: `ENV_NAME_1`

  5. Refer to the env variable in app.js with `process.env.ENV_NAME_1`
  6. Hide the environment information by adding it to gitignore

- Heroku has a Config Variables pane where you input your secrets to Heroku for deployment. Access to this variables is the same with `process.env.ENV_NAME_1`

#### Level 3 Security: Hashing Passwords

- Hash function = No need for encryption key.
- Hash function is fast to calculate forward, but hard to undo.
- To encrypt with hashing, hash the password, and then when logging in, hash the input password and compare the hashes.

  - When you run the hash function on the same string, the hash that is created will always be the same.

- MD5 Node package

  1. `npm i md5`
  2. ` var md5 = require('md5');`
  3. Call with `md5('message');`

---

### Hacking Passwords 101

- (plaintextoffenders.com)[plaintextoffenders.com]

  - Website showing websites that email users their passwords in plaintext

- Hackers create a hash table full of the most common passwords and their hashes and compare them with usernames and their hashed passwords to find matches.
  - These tables exist already on the internet

---

### Salting and hashing passwords with bcrypt

- Salting is adding randomly generated sequence to the user's password before hashing
  - This protects against dictionary attacks; even if the passwords are the same across multiple users, the hashes will not be the same

#### Bcrypt

- Bcrypt is much slower to hash than md5.
  - 20 bil md5 hashes can be generated per second
  - 17,000 bcrypt hashes can be generated per second
- Bcrypt has salt rounds options

  - You can salt then hash multiple times.
  - For every increase in salt rounds, the amount of time to hash the password doubles, allowing you an easy way to keep up with hackers' faster processing speeds.

- node.bcrypt.js Node package

1. Check node version to ensure it is compatible with bcrypt
   - bcrypt does not support unstable versions of node
   - Use nvm if you have a version of node that is not supported
     - nvm is also finicky to install on Mac. If common error is encountered where on nvm install, nvm command is still not found:
       - This fix worked: (https://dev.to/duhbhavesh/nvm-command-not-found-1ho)[https://dev.to/duhbhavesh/nvm-command-not-found-1ho]
       - Supplemental reading on how to use nano editor: (https://linuxize.com/post/how-to-use-nano-text-editor/)[https://linuxize.com/post/how-to-use-nano-text-editor/]
   - Once you have the latest stable version of node installed with `nvm install 14.16.0` (or whatever is the current stable version), look for the compatible version of bcrypt and install with `npm i bcrypt`
   - If issues encountered, install the slightly older version of bcrypt with e.g. `npm i bcrypt@3.0.2`
2. Add to the top of app.js:

   ```
   const bcrypt = require("bcrypt");
   const saltRounds = 10;
   ```

3. There are 2 different ways to generate hashes with bcrypt
   - Version 1 (generate a salt and hash on separate function calls)
   ```
   bcrypt.genSalt(saltRounds, (err, salt) => {
       bcrypt.hash(password, salt, (err, hash) => {
       // store hash in your password db
       });
   });
   ```
   - Version 2 (auto-gen a salt and hash)
     ```
     bcrypt.hash(password, saltRounds, (err, hash) => {
         // Store hash in your password db
     })
     ```

---

```
// This is what we have so far in app.js

//jshint esversion:6
require("dotenv").config();
const express = require("express");
const ejs = require("ejs");
const mongoose = require("mongoose");
// const encrypt = require("mongoose-encryption"); // replaced with md5
// const md5 = require("md5"); // replaced with bcrypt
const bcrypt = require("bcrypt");
const saltRounds = 10;
const app = express();
const port = 3000;

app.use(express.static("public"));
app.set("view engine", "ejs");
app.use(express.urlencoded({ extended: true }));

mongoose.connect("mongodb://localhost:27017/userDB", { useNewUrlParser: true });

const userSchema = {
  email: String,
  password: String,
};

// Must change mongoose schema from above to a fully new one to use encrypt
// const userSchema = new mongoose.Schema({
//   email: String,
//   password: String,
// });

// Using the secret option of encryption
// This will encrypt the entire database by default; here we specify we only want password encrypted and want the username unencrypted for searching
// const secret = "Thisisourlittlesecret."; // This is moved to .env for security
// replaced with md5 hashing below
// userSchema.plugin(encrypt, {
//   secret: process.env.SECRET,
//   encryptedFields: ["password"],
// }); // plugin must be added before you pass userSchema as a parameter below

const User = mongoose.model("User", userSchema);

app.get("/", (req, res) => {
  res.render("home");
});

app.get("/login", (req, res) => {
  res.render("login");
});

app.get("/register", (req, res) => {
  res.render("register");
});

app.post("/register", (req, res) => {
  bcrypt.hash(req.body.password, saltRounds, (err, hash) => {
    const newUser = new User({
      email: req.body.username,
      password: hash, // formerly md5(req.body.password),
    });
    newUser.save((err) => {
      if (err) {
        console.log(err);
      } else if (!err) {
        res.render("secrets"); //only rendered from register and longin routes
      }
    });
  });
});

app.post("/login", (req, res) => {
  const username = req.body.username;
  const password = req.body.password; // formerly md5(req.body.password);
  User.findOne({ email: username }, (err, foundUser) => {
    if (err) {
      console.log(err);
    } else if (!foundUser) {
      res.render("register");
    } else if (foundUser) {
      // if (foundUser.password === password) { //replace with bcrypt
      bcrypt.compare(password, foundUser.password, (err, result) => {
        //comparing the input password with the hash stored in the database under foundUser.password
        if (result) {
          res.render("secrets");
        }
      });
    } else {
      console.log("Invalid username or password.");
    }
  });
});

app.listen(port, () => {
  console.log(`App listening on port ${port}`);
});

```

---

### Cookies and Sessions

- View cookies on your browser under settings
- An HTTP cookie is a small piece of data stored on the user's computer by the web browser while browsing a website. Cookies were designed to be a reliable mechanism for websites to remember stateful information or to record the user's browsing activity. (Wikipedia)[https://en.wikipedia.org/wiki/HTTP_cookie]

#### How cookies work

1. You make request to website
2. Website returns response with a cookie to store some information about the request you made.
   - Cookies are set using the Set-Cookie HTTP header, sent in an HTTP response from the web server. This header instructs the web browser to store the cookie and send it back in future requests to the server (the browser will ignore this header if it does not support cookies or has disabled cookies).
   - e.g.
   ```
   HTTP/1.0 200 OK
   Content-type: text/html
   Set-Cookie: theme=light
   Set-Cookie: sessionToken=abc123; Expires=Wed, 09 Jun 2021 10:18:14 GMT
   ```
   - The value of a cookie may consist of any printable ASCII character (! through ~, Unicode \u0021 through \u007E) excluding , and ; and whitespace characters. The name of a cookie excludes the same characters, as well as =, since that is the delimiter between the name and value.
   - Cookies can also be set by scripting languages such as JavaScript that run within the browser. In JavaScript, the object document.cookie is used for this purpose. For example, the instruction document.cookie = "temperature=20" creates a cookie of name "temperature" and value "20".
3. Cookie is stored in your browser.
   - Sessions cookie are stored until the browser closes. These do not have Expires or Max-Age attributes.
   - Persistent cookies have e.g. Expires attribute which tells the browser to delete it at a specified date and time.
4. When you make another request to the website, the browser sends the cookies in the request header.
   - e.g.
   ```
   GET /spec.html HTTP/1.1
   Host: www.example.org
   Cookie: theme=light; sessionToken=abc123
   ```
   - This gives the server context about the request, allowing persistence of things like shopping carts and logins.

#### Passport.js

Now let's redo app.js with Passport

1. Install:

   - passport
   - passport-local
   - passport-local-mongoose
   - express-session (not express-sessions!)

2. Set up in app.js

   - You can look for this cookie in the browser. This cookie is set to expire when the browsing session ends.

---

### OAuth 2.0

- OAuth is an open standard for token-based authorization
- e.g. Log in with Facebook, google, etc
- Allows us to take advantage of larger companies' complex authentication implementations

#### Why OAuth?

- Allows you to grant a granular level of access
  - e.g. Specify access to profile, and/or email address, and/or friends, etc
- Allows read/write access
  - Can post to Facebook timeline
- Revoke access
  - Third party can revoke access at any time. User can go on Facebook to revoke access.

#### How does OAuth work?

1. Set up app in third party developer console and receive an app ID/client ID.
2. Give user the option to log in via the third party on the third party login page. Redirect.
3. User logs in.
4. User grants permission to access requested.
5. Our app receives authorization code, so we can authenticate them and log them on in our website.
   - We can also exchange this authorization code for an access token from third party and save it in our database, which we can use to keep getting information from the third party.
   - Auth code is a shorter-lived authorization.
   - Access token lasts much longer.

#### Implementing Google OAuth 2.0

1. `npm install passport-google-oauth20`
2. Go to google developer's console and create a project.
   - Go to "Credentials" and configure the OAuth Consent Screen.
3. Create an OAuth Client ID for a web application.
   - For this client ID set your authorized Javascript origins to `http://localhost:3000`
   - Set Authorized redirect URIs to where you want the user to be redirected to after logging in: `http://localhost:3000/auth/google/secrets`
4. Copy Client ID and secret to the .env file.
5. Add to app.js the following

   - `const GoogleStrategy = require('passport-google-oauth20').Strategy;`
   - And set up the strategy:

   ```
   passport.use(new GoogleStrategy({
       clientID: GOOGLE_CLIENT_ID,
       clientSecret: GOOGLE_CLIENT_SECRET,
       callbackURL: "http://localhost:3000/auth/google/secrets",
       userProfileURL: "https://www.googleapis.com/oauth2/v3/userinfo", // where to get profile info

    },
    function(accessToken, refreshToken, profile, cb) {
        User.findOrCreate({ googleId: profile.id}, function(err, user) {
            return cb(err, user);
        });
    }
   ));
   ```

6. .findOrCreate() needs to be implemented.
   - You can quickly do it by installing the mongoose-findOrCreate node module, then adding it to app.js with `const findOrCreate = require('mongoose-findorcreate')`. Note that the variable is spelled exactly the same as the function so that it will be triggered.
   - Add the plugin required:
   ```
   userSchema.plugin(passportLocalMongoose);
   userSchema.plugin(findOrCreate);
   ```
7. Add the "Log in with google" button which looks like this:

```
<div class="col-sm-4">
    <div class="card social-block">
    <div class="card-body">
        <a class="btn btn-block" href="/auth/google" role="button">
        <i class="fab fa-google"></i>
        Sign Up with Google
        </a>
    </div>
    </div>
</div>
```

8. Set up the "/auth/google" route in app.js as well as the callback route when the auth succeeds:

```
// This tells passport to authenticate with the google strategy and specifies we want profile info
// Note it is not in the usual Express syntax
app.get(
"/auth/google",
passport.authenticate("google", { scope: ["profile"] })
);

// This defines what happens when authentication is successful
app.get(
    "/auth/google/secrets",
    passport.authenticate("google", { failureRedirect: "/login" }),
    (req, res) => {
        res.redirect("/secrets");
    }
);
```

9. Set up serialize and deserialize to store the user profile. Note we want to save the user id returned; this is how we will id the user. So we must add googleId to the userSchema.

```
// This code below replaces the above. It works for all kinds of authentication, not just local.
passport.serializeUser((user, done) => {
  done(null, user.id);
});

passport.deserializeUser((id, done) => {
  User.findById(id, (err, user) => {
    done(err, user);
  });
});
```

10. Add social buttons for Bootstrap (optional)
    - Download and drag into public/css folder
    - Add stylesheet above custom stylesheet so you can override if you choose
    - Add classes (e.g. btn-social btn-google) to the buttons

---

### Allowing users to add secrets

1. Create the GET /submit route
2. Check that the user is logged in if trying to access submit (otherwise redirect to /login)
3. If logged in, allow users to POST /submit route. Create that route to get the current user id with req.user.id and find user by ID in the user database, and add the secret to the "secret" attribute as defined in the userSchema (if it hasn't been defined, define it as type String).

```
   if (foundUser) {
   foundUser.secret = submittedSecret;
   foundUser.save(() => {
   res.redirect("/secrets");
   })

```

4. We change /secrets to an open page (no longer requires authentication). Instead it loops through collection of users and checks to see that it is not null, and if so, will post to the page (eventually resulting in a big page full of secrets).

```
app.get("/secrets", (req, res) => {
  User.find({ secret: { $ne: null } }, (err, foundUsers) => {
    if (err) {
      console.log(err);
    } else {
      if (foundUsers) {
        res.render("secrets", { usersWithSecrets: foundUsers });
      }
    }
  });
});
```

5. Change the /secrets ejs page to include a script to render all secrets

```
<% usersWithSecrets.forEach((user) => { %>
    <p class="secret-text"><%=user.secret%></p>
<% }) %>
```

---

### Completed Code Reference

#### app.js

```
//jshint esversion:6
require("dotenv").config();
const express = require("express");
const ejs = require("ejs");
const mongoose = require("mongoose");
const session = require("express-session");
const passport = require("passport");
const passportLocalMongoose = require("passport-local-mongoose"); // passport-local is a dependency, but does not need to be explicitly exposed
const FacebookStrategy = require("passport-facebook").Strategy;
const GoogleStrategy = require("passport-google-oauth20").Strategy;
const findOrCreate = require("mongoose-findorcreate");

const app = express();
const port = 3000;

app.use(express.static("public"));
app.set("view engine", "ejs");
app.use(express.urlencoded({ extended: true }));

// set up passport session
app.use(
  session({
    secret: process.env.SECRET,
    resave: false,
    saveUninitialized: false,
  })
);

// initialize passport and use it to manage sessions
app.use(passport.initialize());
app.use(passport.session());

mongoose.connect("mongodb://localhost:27017/userDB", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});
mongoose.set("useCreateIndex", true);

const userSchema = new mongoose.Schema({
  // note only mongoose schemas can have plugins
  email: String,
  password: String,
  googleId: String,
  facebookId: String,
  secret: String,
});

// add plugin to hash and salt passwords
userSchema.plugin(passportLocalMongoose);
userSchema.plugin(findOrCreate);

const User = mongoose.model("User", userSchema);
passport.use(User.createStrategy());

// The below code only works for local authentication
// passport.serializeUser(User.serializeUser()); // serialise creates the user and stuffs their information into the cookie
// passport.deserializeUser(User.deserializeUser()); // deserialise opens the cookie to read the user information

// This code below replaces the above. It works for all kinds of authentication, not just local.
passport.serializeUser((user, done) => {
  done(null, user.id);
});

passport.deserializeUser((id, done) => {
  User.findById(id, (err, user) => {
    done(err, user);
  });
});

passport.use(
  new FacebookStrategy(
    {
      clientID: process.env.FACEBOOK_CLIENT_ID,
      clientSecret: process.env.FACEBOOK_CLIENT_SECRET,
      callbackURL: "http://localhost:3000/auth/facebook/secrets",
    },
    // here google sends back access token, we will get their profile and use the data they get back to find or create a user in our database
    // The default .findOrCreate() is pseudocode that you can implement or
    // You can use the mongoose package findorcreate which will make it work
    function (accessToken, refreshToken, profile, cb) {
      User.findOrCreate({ facebookId: profile.id }, function (err, user) {
        return cb(err, user);
      });
    }
  )
);

passport.use(
  new GoogleStrategy(
    {
      clientID: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
      callbackURL: "http://localhost:3000/auth/google/secrets",
      userProfileURL: "https://www.googleapis.com/oauth2/v3/userinfo", // where to get profile info
    },
    // here google sends back access token, we will get their profile and use the data they get back to find or create a user in our database
    // The default .findOrCreate() is pseudocode that you can implement or
    // You can use the mongoose package findorcreate which will make it work
    function (accessToken, refreshToken, profile, cb) {
      User.findOrCreate({ googleId: profile.id }, function (err, user) {
        return cb(err, user);
      });
    }
  )
);

app.get("/", (req, res) => {
  res.render("home");
});

app.get(
  "/auth/facebook",
  passport.authenticate("facebook", { scope: ["public-profile", "email"] })
);

app.get(
  "/auth/facebook/secrets",
  passport.authenticate("facebook", {
    successRedirect: "/",
    failureRedirect: "/fail",
  })
);

// This tells passport to authenticate with the google strategy and specifies we want profile info
// Note it is not in the usual Express syntax
app.get(
  "/auth/google",
  passport.authenticate("google", { scope: ["profile"] })
);

// This defines what happens when authentication is successful
app.get(
  "/auth/google/secrets",
  passport.authenticate("google", { failureRedirect: "/login" }),
  (req, res) => {
    res.redirect("/secrets");
  }
);

app.get("/login", (req, res) => {
  res.render("login");
});

app.get("/logout", (req, res) => {
  // de-authenticate user and send them back to home page
  req.logout();
  res.redirect("/");
});

app.get("/register", (req, res) => {
  res.render("register");
});

app.get("/secrets", (req, res) => {
  User.find({ secret: { $ne: null } }, (err, foundUsers) => {
    if (err) {
      console.log(err);
    } else {
      if (foundUsers) {
        res.render("secrets", { usersWithSecrets: foundUsers });
      }
    }
  });
});

// If user is already logged in, render submit page; else redirect to login
app.get("/submit", (req, res) => {
  // Check if logged in, if not must log in
  if (req.isAuthenticated()) {
    res.render("submit");
  } else {
    res.redirect("/login");
  }
});

app.post("/register", (req, res) => {
  User.register(
    { username: req.body.username },
    req.body.password,
    (err, user) => {
      if (err) {
        console.log(err);
        res.redirect("/register");
      } else {
        // passport.authenticate creates a cookie and tells the browser to hold onto it
        passport.authenticate("local")(req, res, () => {
          res.redirect("/secrets");
        });
      }
    }
  );
});

app.post("/login", (req, res) => {
  const user = new User({
    username: req.body.username,
    password: req.body.password,
  });

  req.login(user, (err) => {
    if (err) {
      console.log(err);
    }
    passport.authenticate("local")(req, res, function () {
      res.redirect("/secrets");
    });
  });
});

app.post("/submit", (req, res) => {
  const submittedSecret = req.body.secret;
  // find the current user and save the secret to their file
  // console.log(req.user.id); // Here you see the information req has about the current user
  User.findById(req.user.id, (err, foundUser) => {
    if (err) {
      console.log(err);
    } else {
      if (foundUser) {
        foundUser.secret = submittedSecret;
        foundUser.save(() => {
          res.redirect("/secrets");
        });
      }
    }
  });
});

app.listen(port, () => {
  console.log(`App listening on port ${port}`);
});
```

#### login.ejs

```
<%- include('partials/header') %>

<div class="container mt-5">
  <h1>Login</h1>

  <div class="row">
    <div class="col-sm-8">
      <div class="card">
        <div class="card-body">
          <!-- Makes POST request to /login route -->
          <form action="/login" method="POST">
            <div class="form-group">
              <label for="email">Email</label>
              <input type="email" class="form-control" name="username" />
            </div>
            <div class="form-group">
              <label for="password">Password</label>
              <input type="password" class="form-control" name="password" />
            </div>
            <button type="submit" class="btn btn-dark">Login</button>
          </form>
        </div>
      </div>
    </div>
    <div class="col-sm-4">
      <div class="card social-block">
        <div class="card-body">
          <a class="btn btn-block" href="/auth/facebook" role="button">
            <i class="fab fa-facebook"></i>
            Log In with Facebook
          </a>
        </div>
      </div>
      <div class="card">
        <div class="card-body">
          <a class="btn btn-block" href="/auth/google" role="button">
            <i class="fab fa-google"></i>
            Log In with Google
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

<%- include('partials/footer') %>


```

#### register.ejs

```
<%- include('partials/header') %>

<div class="jumbotron text-center">
  <div class="container">
    <i class="fas fa-key fa-6x"></i>
    <h1 class="display-3">You've Discovered My Secret!</h1>

    <% usersWithSecrets.forEach((user) => { %>
    <p class="secret-text"><%=user.secret%></p>
    <% }) %>

    <hr />

    <a class="btn btn-light btn-lg" href="/logout" role="button">Log Out</a>
    <a class="btn btn-dark btn-lg" href="/submit" role="button"
      >Submit a Secret</a
    >
  </div>
</div>

<%- include('partials/footer') %>
```

#### secrets.ejs

```
<%- include('partials/header') %>

<div class="jumbotron text-center">
  <div class="container">
    <i class="fas fa-key fa-6x"></i>
    <h1 class="display-3">You've Discovered My Secret!</h1>

    <% usersWithSecrets.forEach((user) => { %>
    <p class="secret-text"><%=user.secret%></p>
    <% }) %>

    <hr />

    <a class="btn btn-light btn-lg" href="/logout" role="button">Log Out</a>
    <a class="btn btn-dark btn-lg" href="/submit" role="button"
      >Submit a Secret</a
    >
  </div>
</div>

<%- include('partials/footer') %>

```

#### submit.ejs

```
<%- include('partials/header') %>

<div class="container">
  <div class="jumbotron centered">
    <i class="fas fa-key fa-6x"></i>
    <h1 class="display-3">Secrets</h1>
    <p class="secret-text">Don't keep your secrets, share them anonymously!</p>

    <form action="/submit" method="POST">

      <div class="form-group">
        <input type="text" class="form-control text-center" name="secret" placeholder="What's your secret?">
      </div>
      <button type="submit" class="btn btn-dark">Submit</button>
    </form>


  </div>
</div>
<%- include('partials/footer') %>

```

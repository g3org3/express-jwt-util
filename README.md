# express-auth-jwt 🚓

# a node module

## Getting Started

You will need Node >= 7 installed. [How do I install node? click here to find out about nvm](https://github.com/creationix/nvm#installation)

### Installation

```sh
# install module
npm install express-auth-jwt

# install deps
npm install jsonwebtoken body-parser cookie-parser cookie-session
```

## Contributors

- George <mailto:7jagjag@gmail.com>

## Example

```js
const expressAuthJWT = require("express-auth-jwt");

// Dependencies
const bodyParser = require("body-parser");
const cookieParser = require("cookie-parser");
const session = require("cookie-session");

// Setup
app.use(bodyParser.json());
app.use(cookieParser());
app.use(
  session({
    resave: false,
    secret: "keyboard cat",
    saveUninitialized: true,
    cookie: {
      httpOnly: true,
      maxAge: 3600
    }
  })
);
app.use((req, res, next) => {
  req.jsonResponse = req.headers.accept === "application/json";
  next();
});
const config = {
  database: {
    getUser: Promise.resolve({
      _id: "95fec9bf-5baa-4ccf-aa3b-c0cfea46bdff",
      username: "admin",
      password: "admin",
      name: "admin"
    })
  }
};

// Using the module
const { authEndpoints, authenticate } = expressAuthJWT(config);
app.use(authEnpoints);
app.get("/secret/route", authenticate, (req, res) =>
  res.json("this is private")
);
```

## Default Config

```js
const defaultConfig = {
  database: {
    getUser: () =>
      Promise.reject(
        new Error(
          "Function getUser, not specified in the express-auth-jwt(config)"
        )
      ),
    findQuery: { username: true },
    userIdField: "_id",
    comparePasswords: (dbPassword, requestPassword) =>
      dbPassword === requestPassword
  },
  jwt: {
    secret: "sekret"
  },
  session: {
    cookiePropId: "_id",
    // ---------------- hr | min | sec | mili
    timeToExpireSession: 1 * 60 * 60 * 1000
  },
  messages: {
    tokenNotFound: "No token provided.",
    tokenExpired: "The session has expired.",
    failToAuthenticate: "Could not authenticate.",
    userNotFound: "The user is not valid.",
    wrongPassword: "The password is incorrect.",
    loginSucess: "Enjoy your token!",
    logout: "logout"
  }
};
```

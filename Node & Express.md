src = https://www.freecodecamp.org/learn/back-end-development-and-apis/basic-node-and-express

---

# About Node.js

- Node.js is a **JavaScript runtime** that allows developers to write **backend** (server-side) **programs**.
- Node.js comes with a handful of **built-in modules** (small independent programs)
- some of the **core modules** include:
  - **HTTP**, which acts like a Web server
  - **File System**, a module to read and modify files

---

# Dev Environment - Meet the Node console

Node is just a JavaScript environment. Like client side JavaScript, you can use the console to display useful debug information.  
- On your local machine, you would see console output in a terminal.  
- On Gitpod, a terminal is open at the bottom of the editor by default.

The server must be restarted after making changes to its files.  
You can stop the server from the terminal using Ctrl+C, and start it using Node directly (`node mainEntryFile.js`).  

You can also use a run script in the `package.json` file with `npm run`.  
For example, the `"start": "node server.js"` script would be run from the terminal using `npm run start`.  

To implement **server auto restarting** on file save, Node provides the `--watch` flag you can add to your start script.  
Your `package.json` file would look like this:
```json
{
  ...
  "scripts": {
    "start": "node --watch server.js"
  }
}
```

# Start a working Express server

In the first two lines of the file `myApp.js`, you can see how easy it is to create **an Express app object**: 
```js
let express = require('express');
let app = express();

module.exports = app;
```
The first line **imports the Express.js framework**:
- `require()` is a built-in function in Node.js used to include external modules.
- The `express` variable now contains the Express.js module.

The second line **creates an instance of an Express application**:
- The `express()` function is a top-level function exported by the Express module.
- The `app` variable is now an Express application object.

---

An Express app object has several **methods**.  
One fundamental method is `app.listen(port)`. It tells your server to listen on a given port, putting it in running state.  

---

## Let's serve our first string!  

In Express, routes take the following structure: `app.METHOD(PATH, HANDLER)`  
- `METHOD` is an http method in lower case
- `PATH` is a relative path on the server (it can be a string, or even a regular expression)
- `HANDLER` is a function that Express calls when the route is matched

**Handlers** take the form `function(req, res) {...}` where `req` is the request object and `res` is the response object.  
For example, the following **handler** will serve the string 'Response String':
```js
function(req, res) {
  res.send('Response String');
}
```

---

We can use the `app.get()` method to serve the string "Hello Express" to GET requests matching the `/` (root) path.  
Here's how it looks when applied to `myApp.js`:
```js
let express = require('express');
let app = express();
app.get("/", (req, res) => {
    res.send("Hello Express");
});

module.exports = app;
```
Note that we used an **arrow function** to write the **handler**.

---

## Serve an HTML file

- You can respond to requests with a file, using the `res.sendFile(path)` method  
- You can put it inside the `app.get('/', (re, res) => {...})` route handler

Behind the scenes, this method will set the appropriate **headers** to instruct your browser on how to handle  
the file you want to send, according to its **type**. Then it will read and send the file.  

>[!important]
>This method needs an **absolute file path**.
>We recommend you to use the **Node global variable** `__dirname` to calculate the path like this:
>`absolutePath = __dirname + "/relativePath/file.ext"`

For example, to send the `/views/index.html` file as a response to GET requests to the `/` path, our `myApp.js` file should look like this:
```js
let express = require('express');
let app = express();
let absolutePath = __dirname + "/views/index.html"
app.get("/", (req, res) => {
    res.sendFile(absolutePath);
});

module.exports = app;
```

---

## Serve Static Assets

- An HTML server usually has one or more directories that are accessible by the user.
- You can place there the static assets needed by your application (stylesheets, scripts, images)
- In Express, you can put in place this functionality using the **middleware** `express.static(path)`
- The `path` parameter is the **absolute path** of the folder containing the assets

If you don't know what **middleware** is... don't worry, we will discuss in detail later.
- Basically, middleware are **functions that intercept route handlers**, adding some kind of information
- a middleware needs to be **mounted** using the method `app.use(path, middlewareFunction)` 
- The first `path` argument is optional. If you don't pass it, the middleware will be executed for all requests

**Example**:  
Mount the `express.static()` middleware to the path `/public` with `app.use()`.  
The absolute path to the assets folder is `__dirname + "/public"`
```js
let express = require('express');
let app = express();
let absolutePath = __dirname + '/views/index.html'
app.get("/", (req, res) => {
    res.sendFile(absolutePath);
});
app.use("/public", express.static(__dirname + "/public"));

module.exports = app;
```

---

## Serve JSON on a specific Route

- While an HTML server serves HTML, an **API** serves data.
- A **REST** API allows data exchange in a simple way, without the need for clients to know any detail about the server
- The client only needs to know where the resource is (the **URL**), and the action it wants to perform on it (the **verb**)
- The **GET** verb is used when you are **fetching** some information, without modifying anything
- These days, the preferred data format for moving information around the Web is **JSON**
- Simply put, JSON is a convenient way to represent a JavaScript object as a string, so it can be easily transmitted

>[!note]
>API = Application Programming Interface

>[!note]
>REST = REpresentational State Transfer

### A word about APIs

An API is a set of defined rules and protocols that allows different software applications to communicate with each other.  
APIs serve as **an interface between different software components**, enabling them to:
- Request and exchange data
- Perform operations
- Access services and functionality

In web development, APIs facilitate communication between a client (like a web browser or mobile app) and a server.

### A word about RESTful APIs

REST is an architectural style for designing networked applications.  
RESTful APIs:
- Use standard HTTP methods (GET, POST, PUT, DELETE, etc.)
- Are stateless (the server does not store any information about the client's session state)
- Treat server objects as resources that can be created, read, updated, or deleted

---

When learning to build APIs with Node and Express, you'll encounter:
- **Routing**: Defining URL paths and HTTP methods for API endpoints
- **Middleware**: Functions that have access to the request and response objects
- **Request handling**: Processing incoming data from API calls
- **Response formatting**: Sending data back to the client, often in JSON format
- **Error handling**: Managing and responding to errors in API requests

---

### Let's create a simple API 

We will create a very basic API by creating a route that responds with JSON at the path `/json`.
- You can do it as usual, with the `app.get()` method
- inside the route handler, use the method `res.json()`, passing in an object as an argument
  - this method converts a javaScript object into a string,
  - then sets the appropriate headers to tell your browser that you are serving JSON,
  - and finally sends the data back
- A valid JavaScript object has the usual structrure `{key: data}`
  - `data` can be a number, a string, a nested object, or an array
  - `data` can also be a variable or the result of a function call, in which case it's evaluated before being converted into a string

**Example**:  
Serve the object `{"message": "Hello json"}` as a response, in JSON format, to GET requests to the `/json` route.  
Then point your browser to `your-app-url/json`, the endpoint `/json` should serve the JSON object {"message": "Hello json"}.
```js
let express = require('express');
let app = express();
let absolutePath = __dirname + '/views/index.html'
app.get("/", (req, res) => {
    res.sendFile(absolutePath);
});
app.use("/public", express.static(__dirname + "/public"));
app.get("/json", (req, res) => {
  res.json({"message": "Hello json"})
});

module.exports = app;
```

---

## Use the .env file

- The `.env` file is a hidden file that is used to pass environment variables to your application
- This file is usually a simple text file containing **key-value pairs** where each line represents a single environment variable
  - **KEY**: The name of the environment variable.
  - **VALUE**: The value assigned to that variable. 
- Since `.env` files are designed to be compatible with shell syntax, we generally don't need to wrap names or values in quotes
- This `.env` file is **secret**, no one but you can access it
- and it can be used to store data that you want to keep private or hidden
- For example, you can store **API keys** from external services or your **database URI** (uniform resource identifier)
- You can also use it to store **configuration options**
  - by setting configuration options, you can change the behavior of your app, without the need to rewrite some code

>[!note]
>An API key is a unique identifier for authenticating and authorizing users to an API

>[!note]
>URIs include URLs (uniforme resource locators), which locate and retrieve resources on a network.
>They also include URNs (uniform resource names), which provide a unique name without a means of retrieving the resource

- The environment variables are accessible from the app as `process.env.VAR_NAME`
- The `process.env` object is a global Node object, and variables are passed as strings
- By convention, the environment variable names are all **uppercase**, with words separated by an **underscore**
- It's also important to note that there cannot be space around the equals sign when you assign values to variables
  - _example_: `VAR_NAME=value`

---

### Let's add an environment variable as a configuration option

To do that, we will:
- create an `.env` file to the root of our project directory
- store the variable `MESSAGE_STYLE=uppercase` in this file

```env
MESSAGE_STYLE=uppercase
```

Then, in the `/json` GET route handler we've created earlier, we should: 
- access `process.env.MESSAGE_STYLE` and transform the response object's `message` to uppercase if the variable equals `uppercase`
- At the top of our `myApp.js` file, we need to add `require('dotenv').config()` to load the environment variables.

```js
require('dotenv').config()

let express = require('express');
let app = express();
let absolutePath = __dirname + "/views/index.html"
app.get("/", (req, res) => {
    res.sendFile(absolutePath);
});
app.use("/public", express.static(__dirname + "/public"));
app.get("/json", (req, res) => {
    res.json({"message": "Hello json"});
    // TODO
});

module.exports = app;
```
The response object should either be `{"message": "Hello json"}` or `{"message": "HELLO JSON"}` depending on the `MESSAGE_STYLE` value.  

The `dotenv` package needs to be included in our `package.json` file. It loads environment variables from our `.env` file into `process.env`.  
```json
{
  "name": "fcc-learn-node-with-express",
  "version": "0.1.0",
  "dependencies": {
    "body-parser": "^1.15.2",
    "cookie-parser": "^1.4.3",
    "dotenv": "^16.0.1",
    "express": "^4.14.0",
    "fcc-express-bground": "https://github.com/freeCodeCamp/fcc-express-bground-pkg.git"
  },
  "main": "server.js",
  "scripts": {
    "start": "node --watch server.js"
  }
}
```


---
EOF

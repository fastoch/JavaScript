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

In the context of an Express.js application, `app` typically refers to the main application object created by calling `express()`.  
This object contains all the settings and routes for your web application.  

By assigning `app` to `module.exports`, you're making the Express application instance available to other parts of your program.  
Node.js uses a module system to organize and share code between different files. Each file in Node.js is treated as a separate module.  
Whatever is assigned to `module.exports` becomes available to other files that import (`require`) this module.

---

An Express app object has several **methods**.  
One fundamental method is `app.listen(port)`. It tells your server to listen on a given port, putting it in running state.  

---

# Let's serve our first string!  

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

# Serve an HTML file

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

# Serve Static Assets

- An HTML server usually has one or more directories that are accessible by the user.
- You can place there the static assets needed by your application (stylesheets, scripts, images)
- In Express, you can put in place this functionality using the **middleware** `express.static(path)`
- The `path` parameter is the **absolute path** of the folder containing the assets

If you don't know what **middleware** is... don't worry, we will discuss in detail later.
- Basically, middleware are **functions that intercept route handlers**, adding some kind of information
- a middleware needs to be **mounted** using the method `app.use(path, middlewareFunction)` 
- The first `path` argument is **optional**. If you don't pass it, the middleware will be executed for **all requests**

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

# Serve JSON on a specific Route

- While an HTML server serves HTML, an **API** serves data.
- A **REST** API allows data exchange in a simple way, without the need for clients to know any detail about the server
- The client only needs to know where the resource is (the **URL**) and the action it wants to perform on it (the **verb**)
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

## A word about RESTful APIs

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

## Let's create a simple API 

We will create a very basic API by creating a route that responds with JSON at the path `/json`.
- You can do it as usual, with the `app.get()` method
- inside the route handler, use the method `res.json()`, passing in an object as an argument
  - This method closes the request-response loop, returning the data  
  - Behind the scenes, this method converts a javaScript object into a string
  - then sets the appropriate headers to tell your browser that you are serving JSON,
  - and finally sends the data back
- A valid JavaScript object has the usual structrure `{key: data}`
  - `data` can be a number, a string, a nested object, or an array
  - `data` can also be a variable or the result of a function call, in which case it's evaluated before being converted into a string

**Example**:  
Serve the object `{"message": "Hello json"}` as a response, in JSON format, to GET requests to the `/json` route: 
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
Then point your browser to `your-app-url/json`, the endpoint `/json` should serve the JSON object {"message": "Hello json"}.

---

# Use the .env file

- The `.env` file is a hidden file that is used to pass environment variables to your application
- This file is usually a simple text file containing **key-value pairs** where each line represents a single environment variable
  - **KEY**: The name of the environment variable.
  - **VALUE**: The value assigned to that variable. 
- This `.env` file is **secret**, no one but you can access it, and it can be used to store data that you want to keep private or hidden
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
  - Example: `VAR_NAME=value`
- Since `.env` files are designed to be compatible with shell syntax, we generally don't need to wrap names or values in quotes
- Usually, you will put each variable definition on a separate line

---

## Let's add an environment variable as a configuration option

To do that, we will:
- create an `.env` file to the root of our project directory
- store the variable `MESSAGE_STYLE=uppercase` in this file

Then, in the `/json` GET route handler we've created earlier, we should: 
- access `process.env.MESSAGE_STYLE` and transform the response object's `message` to uppercase if the variable equals `uppercase`
- At the top of our `myApp.js` file, we need to add `require('dotenv').config()` to load the environment variables.

```js
require('dotenv').config();

let express = require('express');
let app = express();

let absolutePath = __dirname + "/views/index.html";

app.get("/", (req,res) => {
    res.sendFile(absolutePath);
});

app.use("/public", express.static(__dirname + "/public"));

app.get("/json", (req, res) => {
    let message = "Hello json";
    const msgCase = process.env.MESSAGE_STYLE;
    if (msgCase === "uppercase") { 
        message = message.toUpperCase();
    }
    console.log(message); // simple check
    res.json({"message": message});
});

module.exports = app;
```
The response object should either be `{"message": "Hello json"}` or `{"message": "HELLO JSON"}`, depending on the `MESSAGE_STYLE` value.  

We also need to include the `dotenv` package in our `package.json` file. It loads the environment variables from our `.env` file into `process.env`.  
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

# Implement a root-level request logger middleware

Earlier, you were introduced to the `express.static()` middleware function.  
Now it's time to see what middleware is, in more detail.  

**Middleware functions** are functions that take 3 arguments: 
- the **request** object 
- the **response** object 
- and the **next function** in the application's **request-response cycle**

These functions execute some code that can have side effects on the app, and usually **add information to the request or response objects**.  
They can also end the cycle by sending a response when some condition is met.  

If they don't send the response when they are done, they start the execution of the next function in the stack.  
This triggers calling the **third argument**, `next()`.

Look at the following example:
```js
function(req, res, next) {
  console.log("I'm a middleware...");
  next();
}
```
Let's suppose we mounted this function on a route. When a request matches the route, it displays the string "I'm a middleware...",  
then it executes the next function in the stack.  

To mount a middleware function at root level, you can use the `app.use(<mware-function>)` method.  
This method executes for all HTTP methods (GET, POST, PUT, etc.), unless a specific path is specified.  
In this case, the function will be executed for all the requests, but you can also set more specific conditions.  
For example, if you want a function to be executed only for POST requests you could use `app.post(<mware-function>)`.  
Analogous methods exist for all the HTTP verbs (GET, DELETE, PUT, ...).  

---

We will build a simple logger. For every request, it will log to the console a string taking the following format:  
`method path - ip`  
An example would look like this: `GET /json - ::ffff:127.0.0.1`  
- You can get the request method (http verb), the relative route path, and the caller's IP from the request object using:
  - `req.method`,
  - `req.path`,
  - and `req.ip`.
- Remember to call `next()` when you are done, or your server will be stuck forever 

>[!note]
>Express evaluates functions in the order they appear in the code. This is true for middleware too.
>If you want a middleware to work for all the routes, it should be mounted before them.

Here's how our `myApp.js` file looks like with the request logger middleware implemented:
```js
require('dotenv').config();

let express = require('express');
let app = express();

app.use("/", (req, res, next) => {
    console.log(`${req.method} ${req.path} - ${req.ip}`); 
    next();
});

let absolutePath = __dirname + "/views/index.html";
app.get("/", (req,res) => {
    res.sendFile(absolutePath);
});

app.use("/public", express.static(__dirname + "/public"));

app.get("/json", (req, res) => {
    let message = "Hello json";
    const msgCase = process.env.MESSAGE_STYLE;
    if (msgCase === "uppercase") { 
        message = message.toUpperCase();
    }
    console.log(message); // simple check
    res.json({"message": message});
});

module.exports = app;
```

---

# Chain middleware to create a time server

- Middleware can be mounted at a specific route using `app.METHOD(path, middlewareFunction)`
- Middleware can also be chained within a route definition

Look at the following example: 
```js
app.get('/user', (req, res, next) => {
  req.user = getTheUserSync(); // Hypothetical synchronous operation
  next();
}, (req, res) => {
  res.send(req.user);
});
```

This approach is useful to **split the server operations into smaller units**.  
That leads to a **better app structure**, and the possibility to **reuse code** in different places.  

This approach can also be used to perform some validation on the data.  
At each point of the middleware stack, you can block the execution of the current chain and pass control to functions  
specifically designed to handle errors. Or you can pass control to the next matching route, to handle special cases.  
We will how in the "Advanced Express" section.

## Exercise

- In the route `app.get('/now', ...)`, chain a middleware function and the final handler.  
- In the middleware function, you should add the current time to the request object in the `req.time` key.  
- You can use `new Date().toString()` to get the current date in string format  
- In the handler, respond with a JSON object taking the structure {time: req.time}.  

## Solution

This time, I won't display the whole `myApp.js` file content, only the block of code that interests us:
```js
app.get('/now', (req, res, next) => {
    req.time = new Date().toString(); 
    next();
  }, (req, res) => {
    res.json({time: req.time});
});
```

---

# Get Route Parameter Input from the client

When building an API, we have to allow users to communicate to us what they want to get from our service.  
For example, if the client is requesting information about a user stored in the database, they need a way to  
let us know which user they're interested in.  
One possible way to achieve this result is by using **route parameters**.  

Route parameters are named segments of the **URL**, delimited by **slashes** `/`.  
Each segment captures the value of the part of the URL which matches its position.  
The captured values can be found in the `req.params` object.  

```js
route_path:'/user/:userId/book/:bookId'
actual_request_URL:'/user/546/book/6754'
req.params:{userId:'546',bookId:'6754'}
```

## Exercise

- Build an echo server, mounted at the route `GET /:word/echo`.  
- Respond with a JSON object taking the structure `{echo: word}`
- You can find the word to be repeated at `req.params.word`
- You can test your route from your browser's address bar by visiting some matching routes
  - e.g.: `your-app-root-path/freecodecamp/echo`
  - in my case: `https://3000-freecodecam-boilerplate-vz45yn8pqnn.ws-eu116.gitpod.io/freecodecamp/echo`
 
## Solution
```js
app.get('/:word/echo', (req, res) => {
  const word = req.params.word; 
  res.json({echo: word});
});
```

---

# Get Query Parameter Input from the Client

Another common way to get input from the client is by encoding the data after the route path, using a **query string**.  
- The query string is delimited by a question mark `?`, and includes `field=value` couples.  
- Each couple is separated by an ampersand `&`
- Express can parse the data from the query string, and populate the object `req.query`
- Some characters, like the percent `%`, cannot be in URLs and have to be encoded in a different format before you can send them
  - If you use the API directly from JavaScript code, you can use specific methods to encode/decode these characters

**Example**:
```js
route_path:'/library'
actual_request_URL:'/library?userId=546&bookId=6754'
req.query:{userId:'546',bookId:'6754'}
```

## Exercise

- Build an API endpoint, mounted at `GET /name`
- Respond with a JSON document taking the structure `{name: 'firstname lastname'}`
- The first and last name parameters should be encoded in a query string, e.g. `?first=firstname&last=lastname`

**NOTE**:  
In the next exercise, you'll receive data from a POST request, at the same `/name` route path.  
If you want, you can use the method `app.route(path).get(handler).post(handler)`.  
This syntax allows you to chain different verb handlers on the same route path.  
This way, you can save a bit of typing, and have cleaner code.

## Solution

```js
app.get('/name', (req, res) => {
    const { first, last } = req.query; // using object destructuring to extract values & assign them to variables
    if (!first || !last) {
        return res.status(400).json({ error: 'Both first and last name are required' });
    }
    res.json({ name:  `${first} ${last}` });
});
```
We can check the result with this URL: https://3000-freecodecam-boilerplate-vz45yn8pqnn.ws-eu116.gitpod.io/name?first=Fabrice&last=Pustoch  
It should print out: `{"name":"Fabrice Pustoch"}`  
**Note**: it would also work with an apostrophe at Pustoc'h

---

# Use body-parser to parse POST requests

- Besides GET, there's another common HTTP verb, it is POST.  
- **POST** is the default method used to send client data with **HTML forms**.
- In REST convention, POST is used to send data to create new items in the database (a new user, a new blog post...)
- we don't have a database in this tutorial, but we still need to learn how to handle POST requests

In this kind of requests, the data doesn't appear in the URL, it is **hidden** in the request body.  
The **body** is a part of the HTTP request, also called the **payload**.  

Even though the data is not visible in the URL, this does not mean that it is private.  
To see why, look at the raw content of an HTTP POST request:
```js
POST /path/subpath HTTP/1.0
From: john@example.com
User-Agent: someBrowser/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 20

name=John+Doe&age=25
```
As you can see in the last line, the body is encoded like the query string. This is the default format used by HTML forms.  
With Ajax, you can also use JSON to handle data having a more complex structure.  

>[!note]
>AJAX = **Asynchronous JavaScript and XML**, a powerful web dev technique that allows web apps to update content dynamically
>without reloading the entire page. https://www.perplexity.ai/search/what-s-ajax-in-web-dev-YouhpBjFQaS8_xJ8y.vYmw

There is another type of encoding: **multipart/form-data**. This one is used to upload **binary files**.  

## Exercise

- in this exercise, you'll use a URL-encoded body
- to parse the data coming from POST requests, you must use the `body-parser` package
- this package allows you to use a series of middleware which can decode data in different formats
- `body-parser` has already been installed and is in your project's `package.json` file
- we need to require this package at the top of `myApp.js` and store it in a variable named `bodyParser`
  - `const bodyParser = require('body-parser');` 
- The middleware to handle URL encoded data is returned by `bodyParser.urlencoded({extended: false})`
- pass the function returned by the previous method call to `app.use()`
- as usual, the middleware must be mounted before all the routes that depend on it

**NOTE**:  
`extended` is a config option that tells `body-parser` which parsing needs to be used.  
- When `extended=false`, it uses the classic encoding `querystring` library.  
- When `extended=true`, it uses `qs` library for parsing.  
When using `extended=false`, values can be only strings or arrays.  
The object returned when using `querystring` does not prototypically inherit from the default JS object, which means  
functions like `hasOwnProperty`, `toString` won't be available.  
The extended version allows more data flexibility, but it is outmatched by **JSON**.

## Solution

Here's the solution:
```js
const bodyParser = require('body-parser'); 
...
app.use(bodyParser.urlencoded({extended: false}));
```

And here's the whole `myApp.js` file:
```js
require('dotenv').config();
const bodyParser = require('body-parser');

let express = require('express');
let app = express();

app.use(bodyParser.urlencoded({extended: false}));

app.use("/", (req, res, next) => {
    console.log(`${req.method} ${req.path} - ${req.ip}`); 
    next();
});

let absolutePath = __dirname + "/views/index.html";
app.get("/", (req,res) => {
    res.sendFile(absolutePath);
});

app.use("/public", express.static(__dirname + "/public"));

app.get("/json", (req, res) => {
    let message = "Hello json";
    const msgCase = process.env.MESSAGE_STYLE;
    if (msgCase === "uppercase") { 
        message = message.toUpperCase();
    }
    console.log(message); // simple check
    res.json({"message": message});
});

app.get('/now', (req, res, next) => {
    req.time = new Date().toString(); 
    next();
  }, (req, res) => {
    res.json({time: req.time});
});

app.get('/:word/echo', (req, res) => {
  const word = req.params.word; 
  res.json({echo: word});
});

app.get('/name', (req, res) => {
    const { first, last } = req.query;
    if (!first || !last) {
        return res.status(400).json({ error: 'Both first and last name are required' });
    }
    res.json({ name:  `${first} ${last}` });
});

module.exports = app;
```

---

# Get Data from POST requests

## Exercise

- Mount a POST handler at the path `/name`
- A form has already been added to the HTML frontpage. It will submit the same data as in 
- 

---
EOF

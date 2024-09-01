src = https://www.freecodecamp.org/learn/back-end-development-and-apis

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

In the first two lines of the file `myApp.js`, you can see how easy it is to create an Express app object: 
```js
let express = require('express');
let app = express();
```
The first line imports the Express.js framework:
- `require()` is a built-in function in Node.js used to include external modules.
- The `express` variable now contains the Express.js module.

The second line creates an instance of an Express application:
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
- Are stateless
- Treat server objects as resources that can be created, read, updated, or deleted

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
  - `data` can also be a variable or the result of a function call, in which case it is evaluated before being converted into a string

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
```


---
EOF

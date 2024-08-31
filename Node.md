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

An Express app object has several methods.  
One fundamental method is `app.listen(port)`. It tells your server to listen on a given port, putting it in running state.  

---

Let's serve our first string!  
In Express, routes take the following structure: `app.METHOD(PATH, HANDLER)`  

...

Here's how it looks when applied to `myApp.js`:
```js
app.get("/", (req, res) => {
    res.send("Hello Express");
});
```

---
EOF

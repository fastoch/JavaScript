# The React, Bun & Hono Tutorial 2024

src = https://www.youtube.com/watch?v=jXyTIQOfTTk&t=89s  

This tutorial is meant to be for any Web Dev, junior or advanced, that wants to learn how to build a modern React App using the following Tech Stack:
- **Bun & Hono** in the backend
- **Vite & React** in the frontend

We're going to develop an **expense tracker** app.

---

# Goal & Web Technologies

Build a full stack modern React App using the following Web technologies:
- In the back-end, we're going to be using **Bun** and the **Hono** framework (which works with all JS runtimes)
- The front-end is going to be a **Vite** + **React** App, which will be **all client-side rendered**
  - **Tailwind** for styling the front-end
  - All of the **TanStack** libraries for querying, routing and forms

All of this will work with React 18 or 19.  

The Front-end is going to make HTTP requests to the Back-end using **Hono RPC** so we get TypeScript safety there.  
**Hono's RPC** (Remote Procedure Call) feature allows sharing API specifications between the server and client, enabling type-safe API calls.  

- The Front-end and the Back-end are both going to use **Zod** for **validation**.
- We're going to use **Kinde Auth** as a fully managed Authentication service
- We're going to set up authorized routes on the front-end and the back-end so only logged in users can perform certain tasks

The database is going to be a relational database, and it doesn't really matter which SQL database you use for this.  
- In this tutorial, we'll be using **Neon PG**, a serverless Postgres database platform
- and we're going to use **Drizzle ORM** since it's the best way to interact with a relational database from a TypeScript application

**NOTE**:  
Towards the end of the video, we'll see a little bit more advanced stuff such as **Frontend caching** & **Optimistic updates** .  

---

## A diagram of our App's architecture

![image](https://github.com/user-attachments/assets/a2eaacc4-bcc3-43ed-adb8-7fec8440b44b)

---

## Bun

Bun is an all-in-one **JavaScript runtime** and **toolkit** that aims to streamline and enhance JavaScript and TypeScript development.  
Bun is a **fast** JavaScript runtime designed as a drop-in replacement for Node.js.  
It's written in **Zig** and powered by **JavaScriptCore**, offering significantly faster startup times and reduced memory usage compared to Node.js.  

- Bun includes a **Node.js-compatible package manager** that's faster than existing tools
- It can **bundle** JavaScript and **TypeScript** code for browsers, Node.js, and other platforms
- Bun can directly execute .jsx, .ts, and .tsx files **without additional configuration**
- Bun provides a **built-in test runner** for executing unit tests
- It supports both **ES** modules and **CommonJS**
- Bun **implements standard Web APIs** like fetch, WebSocket, and ReadableStream

---

## Hono

Hono is a lightweight and ultrafast **web framework** designed for building applications across various JavaScript runtimes, including:
- Cloudflare Workers,
- AWS Lambda,
- Deno,
- Bun,
- and Node.js.

Its primary focus is on performance and simplicity, making it an appealing choice for developers looking to create efficient cloud-native applications.  

Hono boasts a **fast routing mechanism**, utilizing a **RegExpRouter** that performs route matching efficiently without linear loops, which contributes to its speed.  

The framework is minimalistic, with the hono/tiny preset being under 14kB in size and having **zero dependencies**, relying solely on Web Standard APIs.  

Hono includes various **built-in middleware** options, such as **authentication**, **CORS**, and **JSON handling**, which facilitate rapid development.  

The framework offers **first-class TypeScript support**, enhancing developer experience through type safety and improved code quality.  

Hono is designed to provide a **delightful development experience** with clean APIs and comprehensive documentation, making it easier for developers 
to get started and build applications quickly.  

In conclusion, Hono is particularly well-suited for **modern web applications** that require speed and efficiency, making it a **strong alternative to** 
more established frameworks like **Express**.js, especially in **edge computing** scenarios.

---

## TanStack

TanStack Query is a powerful **data fetching** and **state management** library for web applications.  
It is an asynchronous state management library that originated in the React ecosystem.
It provides tools for fetching, caching, synchronizing and updating server state.  

While originally created for React, it now offers adapters for multiple frameworks including React, Vue, Angular and others.

---

# Set up Bun and Hono

We're going to use Bun as the JS runtime, which is a great alternative to Node.js.  
- To install it on Linux: `curl -fsSL https://bun.sh/install | bash`  
- create a new directory for this application: `mkdir app`
- Browse to this new folder: `cd app`
- In here, set up a new Bun server: `bun init`
- Leave settings to defaults
- Open this up in VS Code: `code .`
- The `index.ts` file is going to be the **entry point** of the application
  - to run the app in watch mode, the default cmd is: `bun --watch index.ts` (watch for changes and restart the app)

In the `package.json` file let's create 2 scripts: 
```js
{
  "name": "app",
  "module": "index.ts",
  "type": "module",
  "scripts": {
    "start": "bun index.ts",
    "dev": "bun --watch index.ts"
  },
  "devDependencies": {
    "@types/bun": "latest"
  },
  "peerDependencies": {
    "typescript": "^5.0.0"
  }
}
```
Now, I can run the app in watch mode by using `bun dev`.  

---

To make it an HTTP server (backend app), use `Bun.serve()` in your `index.ts`:
```js
Bun.serve({
  fetch(req) {
    return new Response("Hello from Bun server!"); // visible from a web browser at localhost:3000
  },
});

console.log("Server running!"); // visible from the console
```
This `fetch()` function is going to handle every single HTTP request to our server.  
It doesn't matter what endpoint I go to (in my web browser), it's always going to be served up by that function, and I'll always get the same "Hello from Bun server!" message.  

Obviously, I could write a bunch of `if` statements in here to check what kind of requests we're getting, what the method is, what the URL is...  
But instead, we're going to use a really nice server called **Hono**.  

To do that, we're going to create an `app.ts` file and import the hono module:
```js
import { hono } from 'hono'

const app = new Hono()
//...

export default app
```

You might need to install hono: 
- stop the server with Ctrl+C
- Run `bun install hono`
- restart the server with `bun dev`

Then, in our `index.ts`, we need to import the exported app (since index.tx is the app's entry point):
```js
import app from './app'

Bun.serve({
  fetc: app.fetch;
});

console.log("Server running!"); // visible from the console
```
And as you can see, instead of implementing a custom `fetch()` function, we'll use the built-in Hono `app.fetch` function.  
This way, all HTTP requests will be handled by the Hono library.  

After that, we can set up our endpoints in the usual way in our `app.ts`:
```js
import { hono } from 'hono'

const app = new Hono()

app.get('/test', c => {
  return c.json({"message": "test"})
})

export default app
```
In this example, c is a `Context` object which will be responsible for handling the HTTP request and response.  
So we can get the request data from this object, but also respond with things like JSON or HTML or whatever we want.  

To test the above route, open a web browser and enter this URL: `localhost:3000/test`  
This should display the custom object `{"message": "test}`  

And if we go to a route that we haven't defined, we will get the Hono '404 Not Found' message.  

To know more about this Hono `Context` object: https://hono.dev/docs/api/context  

We can also add a **logger** in our `app.ts` file (https://hono.dev/docs/middleware/builtin/logger):
```js
import { hono } from 'hono';
import { logger } from 'hono/logger';

const app = new Hono()

app.get('/test', (c) => {
  return c.json({"message": "test"})
})

app.use(logger())

export default app
```
We are now able to see (in the console) the request coming in and the response going out:  
![image](https://github.com/user-attachments/assets/04e171a1-04f8-4904-a779-d240470c4f91)  
This is very helpful when we try to debug the HTTP requests.

---

# Adding Routes

- we create a dedicated folder at the root of our project's file tree, and we name it "routes"
- inside this folder, we create a file named `expenses.ts`
- in this file, we will define all the expense routes:
```js
import { hono } from 'hono';

export const expensesRoute = new Hono()
  .get("/", (c) => {
    return c.json({ expenses: [] });
  });
  .post("/", (c) => {
    return c.json({});
  });
  // .delete
  // .put
```

The first endpoint (`.get`) returns an object with the key 'expenses' and then an array of the expenses that we get from the database.  
The second endpoint (`.post`) creates a brand new expense (which is an empty object for now).

Once I've defined all the expense routes in `expenses.ts`, I'm going to import them into `app.ts` with:   
`import { expensesRoute } from './routes/expenses';`

And then, I will have my app serve that up over `app.route("/api/expenses", expensesRoute)`

My `app.ts` now looks like this:
```js
import { hono } from 'hono';
import { logger } from 'hono/logger';
import { expensesRoute } from './routes/expenses';

const app = new Hono()

app.use(logger());

app.get('/test', (c) => {
  return c.json({"message": "test"})
});

app.route("/api/expenses", expensesRoute);

export default app;
```

Now, if I go to localhost:3000/api/expenses, requests will be handled as defined in `expenses.ts`, depending on the type of request (get, post, etc.).  

---

In my `expenses.ts`, I could define the Expense type and add a fakeExpenses array as follows:
```js
import { hono } from 'hono';

type Expense = {
  id: number,
  title: string,
  amount: number
};

const fakeExpenses: Expense[] = [
  { id: 1, title: "Groceries", amount: 50 },
  { id: 2, title: "Utilities", amount: 100 },
  { id: 3, title: "Rent", amount: 1000 }
];

export const expensesRoute = new Hono()
  .get("/", (c) => {
    return c.json({ expenses: fakeExpenses });
  });
  .post("/", async (c) => {
    const expense = await c.req.json();
    return c.json(expense);
  });
  // .delete
  // .put
```
Now, if the client (web browser) makes a GET request, my app will serve up that array of 3 fake expenses.  
And if it makes a POST request, my app will need to insert a new expense into this array.  

The way we insert data in a POST request is through `c.req.json()`, then we store the data into a variable and we return it in JSON format.  
What I expect is for someone to post a new Expense object, but I need a way of checking that the data that is coming over the POST request is actually a valid Expense object.  

And the way we're going to validate this data is by using the **Zod** library, which is pretty much a standard tool for any TypeScript developer.  
Alternative to Zod: https://www.youtube.com/watch?v=nCZ06oegzeM - **Valibot**  





---
@13/218min

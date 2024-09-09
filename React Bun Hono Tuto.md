# The React, Bun & Hono Tutorial 2024

src = https://www.youtube.com/watch?v=jXyTIQOfTTk&t=89s  

---

## Goal & Web Technologies

Build a full stack modern React App using the following Web technologies:
- In the back-end, we're going to be using **Bun** and the **Hono** framework (which works with all JS runtimes)
- The front-end is going to be a **Vite** + **React** App, which will be **all client-side rendered**
  - **Tailwind** for styling the front-end
  - All of the **TanStack** libraries for querying, routing and forms
- All of this will work with React 18 or 19

The Front-end is going to make HTTP requests to the Back-end using Hono RPC so we get TypeScript safety there.  
**Hono's RPC** (Remote Procedure Call) feature allows sharing API specifications between the server and client, enabling type-safe API calls.  

- The Front-end and the Back-end are both going to use **Zod** for **validation**.
- We're going to use **Kinde Auth** as the fully managed Authentication service
- We're going to set up authorized routes on the front-end and the back-end so only logged in users can perform certain tasks

The database is going to be a relational database, and it doesn't really matter which SQL database you use for this.  
- In this tutorial, we'll be using **Neon PG**, a serverless Postgres database platform
- and we're going to use **Drizzle ORM** since it's the best way to interact with a relational database from a TypeScript application

**NOTE**:  
Towards the end of the video, we'll see a little bit more advanced stuff such as **Frontend caching** & **Optimistic updates** .  

---

### A diagram of our App's architecture

![image](https://github.com/user-attachments/assets/a2eaacc4-bcc3-43ed-adb8-7fec8440b44b)

---

### Bun

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

### Hono

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

### TanStack

TanStack Query is a powerful **data fetching** and **state management** library for web applications.  
It is an asynchronous state management library that originated in the React ecosystem.
It provides tools for fetching, caching, synchronizing and updating server state.  

While originally created for React, it now offers adapters for multiple frameworks including React, Vue, Angular and others.

---

## Set up Bun and Hono





---
@2/218min

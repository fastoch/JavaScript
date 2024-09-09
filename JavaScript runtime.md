# What's a JavaScript Runtime?

A JavaScript runtime is a comprehensive **environment that enables the execution of JavaScript code**.  
It consists of **several key components** working together to facilitate the running of JavaScript applications.

---

## Core Components

### JavaScript Engine

This is the heart of the runtime. It's responsible for translating and executing JavaScript code.  
Examples include Google's V8 engine, which powers Chrome and Node.js.

### Web/Global APIs

These are functionalities provided by the runtime environment but not part of the JavaSrcipt language itself.  
They extend JavaScript's capabilities, allowing it to interact with the environment it's running in.  

### Event Loop

This is a crucial part of the runtime that manages the execution of asynchronous code and handles events.

### Callback Queue

Also known as the **task queue**, it holds tasks pushed by Web APIs for execution when the call stack is empty.

### Microtask Queue

This queue handles high-priority tasks like Promise callbacks.

---

## Types of Runtimes

There are 2 main types of JavaScript runtimes.

### Browser runtime

This includes engines like V8 (Chrome), SpiderMonkey (Firefox), 

### Server-side runtime

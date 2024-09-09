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

This includes engines like V8 (Chrome), SpiderMonkey (Firefox), or Chakra (Edge).  
Browser runtimes provide Web APIs for DOM manipulation, AJAX requests, and other browser-specific functionalities.

### Server-side runtime

The most popular is Node.js, which uses the V8 engine but provides different APIs focused on server-side tasks like file system access and network operations.

---

## Key Characteristics

- **Input/Output capabilities**: unlike the core ECMAScript language, which doesn't define I/O, runtimes provide mechanisms for input and output.
- **Event-driven architecture**: most JS runtimes implement an event loop, allowing for non-blocking I/O operations
- **Extensibility**: runtime developers can decide what additional functionality to include, which is why different runtimes may have different APIs or implementations of similar features

---

## Conclusion

Understanding the JS runtime is crucial for developers, as it determines what capabilities are available to their code and how it will be executed in different environments.

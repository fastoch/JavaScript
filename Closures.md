src = https://dev.to/mariaverse/closures-in-javascript-3289

- Closures are a powerful and **fundamental concept** in programming
- Closures are created every time a **function** is created

Here is the official definition of Closures according to MDN (Mozilla Developer Network):  
"A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment)."  

In other words, a closure gives you access to an outer function’s scope from an inner function. 

---

## Closures in JS have three main traits

- a function must return another function
- the inner function has access to variables from the outer function
- variable value from the outer function is persisted

Let's look at an example:
```js
function parentFunc() {
  let counter = 0;

  function childFunc() {
    let innerCounter = 0;
    innerCounter++;
    counter++;
    console.log(counter, innerCounter);
  }

  return childFunc;
}

const myChild = parentFunc();
myChild();
```

Here's what the code does:
- It defines a parent function `parentFunc()` that contains a local variable `counter` and a nested function `childFunc()`
- `childFunc()` has access to both its own local variable `innerCounter` and the outer `counter` variable due to closure
- `parentFunc()` returns `childFunc`, which is then assigned to `myChild`.
- When `myChild()` is called, it executes `childFunc()`, incrementing both `counter` and `innerCounter`, and then logging their values.
- Each time `myChild()` is called, `counter` increases, but `innerCounter` always resets to 1 because it's redeclared each time the function runs


---
EOF

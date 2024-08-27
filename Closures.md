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
- When `myChild()` is called, it executes `childFunc()`, incrementing both `counter` and `innerCounter`, and then logging their values
- Each time `myChild()` is called, `counter` increases, but `innerCounter` always resets to 1 because it's redeclared each time the function runs

The output for the above code is: `1 1`  
But if we execute `myChild()` more than once, let's say 3 times, the output will be:
```
1 1
2 1
3 1
```

- In regular functions, once the function is executed it is wiped clean and does not persist any data.
- When using closures however, the inner function stores variables from the outer function in its own special location
- `childFunc()` persists the value for `counter` as it is when the function completes
- meaning that at the start of the second execution, `counter` is not set to 0, it is set to 1
- and at the start of the third execution, `counter` is set to 2

This makes the use of closures a great option when we want to remember certain values in functions.

---
EOF

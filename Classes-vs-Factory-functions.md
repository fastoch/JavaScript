src = https://dev.to/snevy1/classes-vs-factory-functions-in-javascript-4fjn  

---

In JavaScript, you can arrange your code using object-oriented patterns such as factory functions, classes and constructors.  
Choosing the right pattern can be tricky.

---

# Classes in JavaScript

If you know constructors then classes shouldn't be hard to grasp.  
Classes are a syntactic sugar of constructors in Javascript.  

## Constructor function

The code below shows a constructor function:
```js
function playerCreator(name, score) {
  this.name = name;
  this.score = score;
}

playerCreator.prototype.increment = function() {
  this.score++;
};
playerCreator.prototype.login = function() {
  this.login = false;
};
```

Notice how the **increment** and **login** functions are placed on the prototype.  
They are **shared functions**, so any object created by the constructor has access to these methods. This saves memory.  

## Class object

This next code shows a class object:
```js
class playerCreator {

  constructor(name, score){
    this.name = name;
    this.score = score;
  }

  increment: function() {
    this.score++;
  };
  login: function() {
    this.login = false;
  };
}
```

When we create a class object, 

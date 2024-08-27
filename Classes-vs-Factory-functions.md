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

When we create a class object, we start with the keywork **class** then the function name.  
Inside of the function, we add a constructor that holds the parameters to be used.  
Notice that instead of adding **increment** and **login** methods on the prototype explicitly, we nicely tuck them inside the class object,  
though under the hood they are added to the prototype (implicitly).  

## Why to use Classes?

So if constructors and classes do essentially the same thing, why should we use classes instead of constructors?  
- Classes are easy to read
- By using classes, Javascript tries to follow the normal convention of object-oriented programming (OOP), though it is still
  a prototypical-based language.

Classes are not native to Javascript but are widely used in OOP languages such as C# and Java.  
To avoid confusion that developers from these languages face when they encounter Javascript, classes are a better solution.

---

# Factory functions

- Factory functions are functions that return a new object.
- They're quite similar to classes, but they utilize the power of **closures**.
- once you call the function, it sets up and returns a new object

Unlike constructor functions or classes, **factory functions** don't require the '**new**' keyword and **can return any type of object**,  
providing **more flexibility in object creation**.

>[!important]
>What are **closures**? => https://github.com/fastoch/JavaScript/blob/main/Closures.md

Below is how we would implement a factory function:
```js
function playerCreator(name, score) {
  return {
    name: name,
    score: score,
    isLoggedIn: false,  // Add a property to track login status
    increment() {
      return (this.score += 1);
    },
    login() {
      this.isLoggedIn = true;  // Set login status to true
      return this.isLoggedIn;
    },
    logout() {
      this.isLoggedIn = false;  // Add a logout method
      return this.isLoggedIn;
    }
  };
}

let player1 = playerCreator("John", 8);
player1.increment(); // 9
```

## Benefits of using Factory Functions over Classes 

- There is no use of the **this** keyword when instantiating variables, it is only used inside methods.
- There is no use of the **new** keyword which is often forgotten when creating objects using classes.
- It's easier to set up since it looks quite similar to regular functions
- **Simplicity**:
  - factory functions provide a more straightforward and flexible way to create objects,
  - they don't have the syntactic complexities that come with class declarations and prototypes,
  - making them more approachable for developers who prefer a simpler syntax
- **Encapsulation with Closures**:
  - it's easy to encapsulate private variables and objects with factory functions since it's inherent in its use

## Benefits of using Classes over Factory Functions

- Classes save memory, so they are suitable when creating large applications that require creation of thousands of objects
- Classes are used in many programming languages hence it is a cleaner way to follow object-oriented programming paradigm
- **Inheritance**:
  - classes support the concept of inheritance, making it easier to create a hierarchy of objects
  - you can use the **extends** keyword to create a **subclass** that inherits from a **superclass**
```js
class Student extentds Person {
  constructor(name, age, grade) {
    super(name, age);
    this.grade = grade;
  }

  // Additional methods or overrides
}

const student1 = new Student('James', 18, 'A');
```
- **'instanceof'** operator:
  - classes make use of the **instanceof** operator, which can be useful for type-checking and determining if an object
    is an instance of a particular class
```js
console.log(student1 instanceof Student); // true
console.log(student1 instanceof Person); // true
```

# Conclusion

The choice between classes and factory functions depends on your specific needs and which pattern your are comfortable with.  

---
EOF

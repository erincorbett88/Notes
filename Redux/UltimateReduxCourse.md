# The Ultimate Redux Course
___
Beginner-friendly introduction to Redux.

This course is a Code With Mosh - [The Ultimate Redux Course](https://members.codewithmosh.com/courses/enrolled/783424).

- Redux is a state-management library for JavaScript applications. Can be used with React, Vue, Angular... doesn't care what library we use.
- There are other state management systems out there - Facebook uses Flux, and Google uses MobX. Redux grew out of Flux.
- All the state is stored inside a "store" - a central repository. So UI components don't manage their own state.
- Pros of Redux:
  - predictable state changes
  - centralized state
  - easy debugging
  - preserve page state
  - undo/redo pages
- Cons of Redux:
  - Adds complexity to code ("functional programming")
  - verbose/some boilerplate code

## Functional Programming in JavaScript
___

### What is Functional Programming?

(as opposed to object-oriented programming or event-driven programming)

- invented in the 1950s but is becoming trendy
- decompose problems into smaller functions - take some input and return an output
- doesn't mutate or change data

Benefits: more concise, easier to debug/test, more scalable

Cons: less intuitive for some people, more difficult to learn

### Functions as First-Class Citizens

Functions are considered **first class citizens**, which means they can be treated just like a variable. For example, they can be passed as arguments to other functions. For example:
```
function sayHello() {
  return 'Hello, World!';
}

let fn = sayHello;
```

Here, fn doesn't _call_ sayHello or get its return variable, it just references it or is an "alias" for it. So we can pass fn to another function, or assign it to a variable.  We can call sayHello by using _fn()_.

If we wanted to _call_ sayHello, we would have declared _let fn = sayHello();_.

```
function sayHello() {
  return function() {
    return 'Hello, World!';
  }
}

let fn = sayHello();
let message = fn()
```
The above code does something different. When we initialize fn, we _call_ sayHello(), but _not_ the anonymous function that it returns. so sayHello() returns, like, the outer layer of the return - just the function. When we initialize message and it calls fn(), this is when we get to access the inner layer of that return statement.

### Higher-order Functions

```
function greet(fn) {
  console.log(fn());
}

function sayHello() {
  return function() {
    return 'Hello, World!';
  }
}
```
Two different examples: in first, we are passing a function into another function. In the second, we are passing a function that returns another function. So we can pass functions as arguments to other functions, and we can return functions from other functions. These are both examples of a **higher-order function**, which does one or both of these things. Higher-order functions are a key concept in functional programming..

### Function Composition

So if the idea of functional programming is to write a bunch of small and reuasable functions and compose them together, we can do that with function composition.
```
const trim = str = > str.trim();
const wrapInDiv = str = > `<div>${str}</div>`;

const input = '  Hello, World!  ';
const result = wrapInDiv(trim(input));
```
_Result_ here is an example of function composition.

### Composing and Piping

Lodash has a built in "compose" function that works like this:
```
const transform = compose(wrapInDiv, toLowerCase, trim)
transform(input);
```

Note that transform doesn't _call_ those three functions, just references them. Doesn't call them until we call transform. So we can compose functions together and then call them all at once. This is called **function composition**.

We can also use **piping** to do the same thing. Piping is similar to function composition, but it passes the result of one function into the next function. So we can use _pipe_ instead of _compose_.
```
const transform = pipe(trim, toLowerCase, wrapInDiv)
transform(input);
```
That changes the order of the functions, so we can use _pipe_ or _compose_ depending on how we want to order the functions. In this case, it doesn't matter because they are all independent of each other. Pipe lets us read them left to right. Using compose, we have to read them right to left. So it's a matter of preference.

### Currying

Currying is a technique in functional programming where we take a function that takes multiple arguments and turn it into a function, or a series of functions, that each take one argument.

To write a function to add a + b, we would normally write
```
function add(a, b) {
  return a + b;
}
```
But we can also write it like this:
```
function add(a) {
  return function(b) {
    return a + b;
  }
}
```
Now, when we call the function, instead of ```add(1, 2)```, we can call it using ```add(1)(2)```. The first call returns a function that takes b as an argument, and the second call passes in b. So we can call add(1) and get a function back, and then call that function with 2. This is called **currying**.

This can also be done with arrow functions. Normally we would write
```
const add = (a, b) => a + b;
```
But we can also write it like this:
```
const add = a => b => a + b;
```
### Pure Functions

A function is pure if, when it is given the same input, it always returns the _same result_.
Some rules:
- No random values
- No current date/time
- No global state (DOM, files, db, etc.)
- No side effects (no changing the input, no changing the state of the program)
- No mutations (no changing the input, no changing the state of the program)

Not every function _needs_ to be pure.
But pure functions are easier to test, easier to debug, and easier to reason about. So we should try to write pure functions whenever possible.
```
function isOldEnough(age) {
  return age >= minAge;
}
``` 
Here, we're relying on the minAge global variable unchanging. Alternatively:
```
function isOldEnough(age, minAge) {
  return age >= minAge;
}
```
This is a pure function because it doesn't rely on any external state. It takes in two arguments and returns a value based on those arguments. So we can test it easily. And as long as we pass in the same two values, it will always return the same result.

### Immutability
Immutability is the idea that we don't change the state of an object. Instead, we create a new object with the new state. This is important in functional programming because it helps us avoid side effects and makes our code easier to reason about.

If we want to change an object, we create a copy and change the copy. For example:
```
string Name = 'John';
string newName = Name + ' Doe';
```
Here, we create a new string called newName that is a copy of Name with ' Doe' added to it. We don't change the original string. This is an example of immutability.
In contrast, if we had written:
```
string Name = 'John';
Name += ' Doe';
```
This would change the original string, which is not immutable. So we should always try to use immutability in our code.

In JavaScript, objects and arrays are _not_ immutable - it is not designed to be a "functional" language, but is rather a "multi-paradigm" language. So if you want to apply this concept, you have to do it on your own.

### Updating Objects
Say we have a book.
```
let book = {};
book.title = 'Harry Potter';
book.author = 'J.K. Rowling';
```
We can directly change the book object:
```
book.title = 'Harry Potter and the Sorcerer\'s Stone';
```
This is not immutable. Instead, we should create a new object. One way to update objects is with Object.assign:
```
newBook = Object.assign({}, book, { title: 'Harry Potter and the Sorcerer\'s Stone' });
```
Another way is to use the spread operator (more common in modern JavaScript and React):
```
let first* Book = {
  ...book,
  title: 'Harry Potter and the Sorcerer\'s Stone'
};
```
Now, when updating objects, we have eto make sure not to provide a "shallow" copy. If we have a nested object, we need to make sure to create a new copy of the nested object as well. For example:
```
let book = {
  name: "Harry Potter",
  author: "J.K. Rowling",
  publisher: {
    name: "Scholastic",
    address: "123 Main St"
  }
};
``` 
If we create a new object using the spread operator, it will only create a shallow copy of the book object. So if we change the publisher name, it will also change the original book object:
```
const newBook = {
  ...book,
  name: "Harry Potter and the Sorcerer's Stone"
};

newBook.publisher.name = "Random House";
```
If we do this, then in the ORIGINAL book object, the book publisher name will be "Random House". So we need to make sure to create a new copy of the nested object as well. This is called "deep cloning" or "deep copying". There are libraries that can help with this, like lodash's cloneDeep function. Or, we could use a nested spread operator:
```
const newBook = {
  ...book,
  publisher: {
    ...book.publisher,
    name: "Random House"
  }
};
```
### Updating Arrays
```
const numbers = [1, 2, 3, 4, 5];
```
**Adding:**

Add using spread operator:
```
const added = [4, ...numbers];
```
or if you want it added in a particular place:
```
const index = numbers.indexOf(3);
const added = [
  ...numbers.slice(0, index), 
  4, 
  ...numbers.slice(index)
];
```
**Removing:**
```
const removed = numbers.filter(n => n !== 3);
```
**Updating:**
```
const updated = numbers.map(n => n===2 ? 20 : n);
```
We map over the array and return a new array where the only changed value is changed where n=2. If we are working with objects instead of numbers, then we have to use the spread operator to provide a full copy of the object.
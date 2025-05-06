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
(as opposed to object-oriented programming or event-driven programming)

- invented in 1950s but is becoming trendy
- decompose problems into smaller functions - take some input and return an output

Functions are considered **first class citizens** - can be passed as arguments to other functions. For example:
```
function sayHello() {
  return 'Hello, World!';
}

let fn = sayHello;
```

Here, fn doesn't _call_ sayHello, it just references it or is an "alias" for it. So we can pass fn to another function, or assign it to a variable.
If we wanted to _call_ sayHello, we would have declared _let fn = sayHello();_. We can call sayHello by using _fn()_.

### What is Functional Programming?

### Functions as First-Class Citizens

### Higher-order Functions


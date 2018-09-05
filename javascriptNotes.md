### Scope in Javascript
#### Function scope
A function scope is created for every function.
```javascript
function iHaveScope() {
  // local function scope
  function iHaveNestedScope() {
    // nested local function scope
  }
}
```

#### Block scope
Block scopes are what you get when you use **_if statements, for statements,_** and the like. You can also use them stand-alone with a simple begin-end curly braces {}, not to be confused with empty object literals.
```javascript
var a = {} // empty object literal
{ var a } // undefined object in a block scope
if (3 == '3') {
  // block scope for the if statement
}
for (var i=0; i<10; i++) {
  // block scope for the for statement
}
```

#### var vs let vs const behavior in scope
**_var_** behaves differently in block scope and functional scope.
A variable declared with var in a block scope is available outside of that block scope.
```javascript
for (var i=0; i<10; i++) {
  // block scope for the for statement
}
console.log(i) // => 10 (why oh why)
```
A variable declared with var in a function scope can’t be accessed outside that function scope.
```javascript
function iHaveScope() {
  var secret = 42;
}
secret; // ReferenceError: secret is not defined (in this scope)
```

**_let_** behaves differently for block scope than **_var_**. Variables declared with **_let_** inside a block scope are only accessible inside that scope, making **_let_** the ideal solution to the for loop index variable scope problem. 
If we use let to declare the i variable in a for loop, that variable will only be available inside the for loop.
```javascript
for (let i=0; i<10; i++) {
  // block scope for the for statement
}
console.log(i) // ReferenceError: i is not defined (D'oh!)
```
##### const
Declaring a variable with **_const_** is exactly like let - when it comes to scopes - but creates a constant reference for the variable. We can’t change the value of a constant reference. 
If we put a primitive value in a constant, that value will be protected from getting changed:
```javascript
const PI = 3.141592653589793
PI = 42 // SyntaxError: "PI" is read-only
```
Note that if the constant is an object, we can still change the properties of that object, so be careful about that:
```javascript
const dessert = { type: 'pie' };
dessert.type = 'pudding'; // Sure thing
console.log(dessert.type) // pudding
```
We can’t however, reassign an object declared with const:
```javascript
const dessert = { type: 'pie' };
dessert = { type: 'cake' }; // SyntaxError: "dessert" is read-only
```
If we want a completely immutable object, we’ll have to use something else like library - Immutable.js.

### Closure
When code is run in JavaScript, the environment in which it is executed is very important, and is evaluated as 1 of the following:
**_Global code—_** The default environment where your code is executed for the first time.
**_Function code—_** Whenever the flow of execution enters a function body.

(…), let’s think of the term execution context as the environment / scope the current code is being evaluated in.
In other words, as we start the program, we start in the global execution context. Some variables are declared within the global execution context. We call these global variables. When the program calls a function, what happens? A few steps:
1. JavaScript creates a new execution context, a local execution context.
2. That local execution context will have its own set of variables, these variables will be local to that execution context.
3. The new execution context is thrown onto the execution stack. Think of the execution stack as a mechanism to keep track of where the program is in its execution.

When does the function end? When it encounters a return statement or it encounters a closing bracket }. When a function ends, the following happens:
1. The local execution contexts pops off the execution stack
2. The functions sends the return value back to the calling context. The calling context is the execution context that called this function, it could be the global execution context or another local execution context. It is up to the calling execution context to deal with the return value at that point. The returned value could be an object, an array, a function, a boolean, anything really. 
3. If the function has no return statement, undefined is returned.
4. The local execution context is destroyed. This is important. Destroyed. All the variables that were declared within the local execution context are erased. They are no longer available. That’s why they’re called local variables.

**_Closure_**
Whenever you declare a new function and assign it to a variable, you store the function definition, as well as a closure. The closure contains all the variables that are in scope at the time of creation of the function. It is analogous to a backpack. A function definition comes with a little backpack. And in its pack it stores all the variables that were in scope at the time that the function definition was created.
> The closure is a collection of all the variables in scope at the time of creation of the function.

When a function returns a function, that is when the concept of closures becomes more relevant. The returned function has access to variables that are not in the global scope, but they solely exist in its closure.

> A **_closure_** is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). 

> In other words, a **_closure_** gives you access to an outer function’s scope from an inner function. 

> In JavaScript, **_closure_** are created every time a function is created, at function creation time.

To use a **_closure_**, simply define a function inside another function and expose it. To expose a function, return it or pass it to another function.

The inner function will have access to the variables in the outer function scope, even after the outer function has returned.

#### Usage of Closure:
Data privacy OR Encapsulation
In functional programming, closures are frequently used for partial application & currying
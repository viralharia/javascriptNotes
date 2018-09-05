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
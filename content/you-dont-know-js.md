# Learning journal of You Don't Know JS
[Free Book Online Book](https://github.com/getify/You-Dont-Know-JS)

## Up & Going

## Scope & Closures

## this & Object Prototypes

## Types & Grammar

## Async & Performance

## ES6 & Beyond
#### Unlink var, let doesn't do hoisting
```javascript
{
  console.log( a );   // undefined
  console.log( b );   // ReferenceError!
  var a;
  let b;
}
```
#### ReferenceError
This ReferenceError from accessing too-early let-declared references is technically called a Temporal Dead Zone error -- you're accessing a variable that's been declared but not yet initialized.

#### let + for
```javascript
var funcs = [];

for (let i = 0; i < 5; i++) {
    funcs.push( function(){
        console.log( i );
    } );
}

funcs[3]();     // 3
```
The `let i` redeclares a new i for each iteration of the loop. If use var, not one copy of i is created. So the result of last line will be 5.
```javascript
funcs[3]();     // 5
```

#### Block-scoped Functions
```javascript
if (something) {
  function foo() {
    console.log("1");
  }
} else {
  function foo() {
    console.log("2");
  }
}

foo()
```
In pre-ES6 environments, foo() would print "2" regardless of the value something, because both function declarations were hoisted out of blocks, and the second one always wins.
In ES6, that last line throws a ReferenceError

#### Default Value Expressions
Formal parameters in a function declaration are in their own scope, not in the function body's scope. Reference to an identifier in a default value expression first matches the formal parameters' scope before looking to an outer scope.

A default value expression can even be an inline function expression call.

```javascript
function foo( x =
    (function(v){ return v + 11; })( 31 )
) {
    console.log( x );
}

foo();          // 42
```

#### Destructuring
```javascript
var a, b, c, x, y, z;

[a,b,c] = foo();
( { x, y, z } = bar() );

console.log( a, b, c );             // 1 2 3
console.log( x, y, z );             // 4 5 6
```

`Note`: For the object destructuring form specifically, when leaving off a var/let/const declarator, we had to surround the whole assignment expression in (), because otherwise the  {...} on the lefthand side as the first element in the statement is taken to be a block statement instead of an object.

[Destructuring Assignment Expressions](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20&%20beyond/ch2.md#destructuring-assignment-expressions)
By carrying the object/array value through as the completion, you can chain destructuring assignment expressions together.

[DEstructuring Defaults + Parameter Defaults](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20&%20beyond/ch2.md#destructuring-defaults--parameter-defaults)

```javascript
function f6({ x = 10 } = {}, { y } = { y: 10 }) {
    console.log( x, y );
}

f6();                               // 10 10
f6( undefined, undefined );         // 10 10
f6( {}, undefined );                // 10 10

f6( {}, {} );                       // 10 undefined
f6( undefined, {} );                // 10 undefined

f6( { x: 2 }, { y: 3 } );           // 2 3
```

:bookmark:
[Default Value Assignment](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20&%20beyond/ch2.md#default-value-assignment)

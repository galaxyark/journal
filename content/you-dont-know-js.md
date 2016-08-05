# Learning journal of You Don't Know JS
[Online Book](https://github.com/getify/You-Dont-Know-JS)

## Up & Going

## Scope & Closures

## this & Object Prototypes

## Types & Grammar

## Async & Performance

## ES6 & Beyond

### Chapter 2: Syntax

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

#### Concise Methods
```javascript
var o = {
    x() {
        // ..
    },
    y() {
        // ..
    }
}
```
Thie concise methods definition implies `something: function(x, y)`. Concise methods imply anonymous function expressions.

##### Concisely Unnamed
```javascript
runSomething( {
    something(x,y) {
        if (x > y) {
            return something( y, x );
        }

        return y - x;
    }
} );
```
Use named function for a better recursion. This could avoid ugly code like `let self = this`

#### Computed Property Names [..]
```javascript
let prefix = "user_";
let o = {
  baz: function(..){..},
  [ prefix + "foo"]: function(..){..},
  [ prefix + "boo"]: function(..){..}
};
```

#### Warning
As a word of caution, be very careful about the readability of your code with such new found power. Just like with default value expressions and destructuring assignment expressions, just because you can do something doesn't mean you should do it. Never go so overboard with new ES6 tricks that your code becomes more clever than you or your other team members.

#### Tagged Template Literals
```javascript
function foo(strings, ...values) {
    console.log( strings );
    console.log( values );
}

function bar() {
    return function foo(strings, ...values) {
        console.log( strings );
        console.log( values );
    }
}

var desc = "awesome";

foo`Everything is ${desc}!`;
// [ "Everything is ", "!"]
// [ "awesome" ]
bar()`Everything is ${desc}!`;
```

#### Arrow Functions
```javascript
var controller = {
    makeRequest: (..) => {
        // ..
        this.helper(..);
    },
    helper: (..) => {
        // ..
    }
};

controller.makeRequest(..);
```
`this.helper()` reference fails, because `this` here doesn't point to controller. It lexically inherits `this` from the surrounding scope. In this example, it is global.

So now we can conclude a more nuanced set of rules for when => is appropriate and not:

- If you have a short, single-statement inline function expression, where the only statement is a return of some computed value, and that function doesn't already make a this reference inside it, and there's no self-reference (recursion, event binding/unbinding), and you don't reasonably expect the function to ever be that way, you can probably safely refactor it to be an => arrow function.
- If you have an inner function expression that's relying on a var self = this hack or a .bind(this) call on it in the enclosing function to ensure proper this binding, that inner function expression can probably safely become an => arrow function.
- If you have an inner function expression that's relying on something like var args = Array.prototype.slice.call(arguments) in the enclosing function to make a lexical copy of arguments, that inner function expression can probably safely become an => arrow function.
- For everything else -- normal function declarations, longer multistatement function expressions, functions that need a lexical name identifier self-reference (recursion, etc.), and any other function that doesn't fit the previous characteristics -- you should probably avoid => function syntax.

Bottom line: => is about lexical binding of this, arguments, and super. These are intentional features designed to fix some common problems, not bugs, quirks, or mistakes in ES6.

#### Number Literal Extensions
```javascript
var dec = 42,
    oct = 0o52,         // or `0O52` :(
    hex = 0x2a,         // or `0X2a` :/
    bin = 0b101010;     // or `0B101010` :/
```

#### Regular expressions
#### Unicode

#### Symbol
`Symbol`
`Symbol.for`

##### Symbol as Object Properites
```javascript
var o = {
    foo: 42,
    [ Symbol( "bar" ) ]: "hello world",
    baz: true
};

Object.getOwnPropertyNames( o );    // [ "foo","baz" ]
Object.getOwnPropertySymbols( o );  // [ Symbol(bar) ]
```

### chapter 3: Organization
#### iterators
```
Iterator [optional]
    return() {method}: stops iterator and returns IteratorResult
    throw() {method}: signals error and returns IteratorResult

IteratorResult
    value {property}: current iteration value or final return value
        (optional if `undefined`)
    done {property}: boolean, indicates completion status

Iterable
    @@iterator() {method}: produces an Iterator
```

Use iterators to organize not only data, but also functions. Yeah, JavaScript is a functional programming language.
```javascript
var tasks = {
    [Symbol.iterator]() {
        var steps = this.actions.slice();

        return {
            // make the iterator an iterable
            [Symbol.iterator]() { return this; },

            next(...args) {
                if (steps.length > 0) {
                    let res = steps.shift()( ...args );
                    return { value: res, done: false };
                }
                else {
                    return { done: true }
                }
            },

            return(v) {
                steps.length = 0;
                return { value: v, done: true };
            }
        };
    },
    actions: []
};

tasks.actions.push(
    function step1(x){
        console.log( "step 1:", x );
        return x * 2;
    },
    function step2(x,y){
        console.log( "step 2:", x, y );
        return x + (y * 2);
    },
    function step3(x,y,z){
        console.log( "step 3:", x, y, z );
        return (x * y) + z;
    }
);

var it = tasks[Symbol.iterator]();

it.next( 10 );          // step 1: 10
                        // { value:   20, done: false }

it.next( 20, 50 );      // step 2: 20 50
                        // { value:  120, done: false }

it.next( 20, 50, 120 ); // step 3: 20 50 120
                        // { value: 1120, done: false }

it.next();              // { done: true }
```

#### Generators(TODO LATER)
* Producing a series of values: This usage can be simple (e.g., random strings or incremented numbers), or it can represent more structured data access (e.g., iterating over rows returned from a database query).

  Either way, we use the iterator to control a generator so that some logic can be invoked for each call to next(..). Normal iterators on data structures merely pull values without any controlling logic.

* Queue of tasks to perform serially: This usage often represents flow control for the steps in an algorithm, where each step requires retrieval of data from some external source. The fulfillment of each piece of data may be immediate, or may be asynchronously delayed.

  From the perspective of the code inside the generator, the details of sync or async at a yield point are entirely opaque. Moreover, these details are intentionally abstracted away, such as not to obscure the natural sequential expression of steps with such implementation complications. Abstraction also means the implementations can be swapped/refactored often without touching the code in the generator at all.

When generators are viewed in light of these uses, they become a lot more than just a different or nicer syntax for a manual state machine. They are a powerful abstraction tool for organizing and controlling orderly production and consumption of data.

```javascript
function *foo(x, y) {
    // ..
}

foo(5, 10);
```




### chapter 4: Async Flow Control
### chapter 5: Collections
### chapter 6: API Additions
### chapter 7: Meta Programming
### chapter 8: Beyond ES6


:bookmark:
[Iterators](https://github.com/getify/You-Dont-Know-JS/blob/4850bcaf4e1a1abe4dc64fae846cb5235d8f4c18/es6%20%26%20beyond/ch3.md#iterators)

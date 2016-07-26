# Learning journal of You Don't Know JS
[Link](https://github.com/getify/You-Dont-Know-JS)

## Up & Going

## Scop & Closures

## this & Object Prototypes

## Types & Grammar

## Async & Performance

## ES6 & Beyond
#### Unlink var, let doesn't do hoisting

    {
    console.log( a );   // undefined
    console.log( b );   // ReferenceError!
    var a;
    let b;
    }

#### ReferenceError
This ReferenceError from accessing too-early let-declared references is technically called a Temporal Dead Zone error -- you're accessing a variable that's been declared but not yet initialized.

#### let + for
    var funcs = [];

    for (let i = 0; i < 5; i++) {
        funcs.push( function(){
            console.log( i );
        } );
    }

    funcs[3]();     // 3

The `let i` redeclares a new i for each iteration of the loop. If use var, not one copy of i is created. So the result of last line will be 5.

    funcs[3]();     // 5

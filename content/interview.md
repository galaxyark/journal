### front end
#### Resources
* [Cracking the front-end interview](https://medium.freecodecamp.com/cracking-the-front-end-interview-9a34cd46237#.4w3f64tjl)
* [Front-end-Developer-interview-questions](https://github.com/h5bp/Front-end-Developer-Interview-Questions)

#### To Prepare
* Radio button, Check box, Selected
* Form, Figure
* box-shadow
* media query
* HTML & CSS small example:
  * Tooltip
  * Dropdown Menu
  * Calendar
  * Hover on a button to change content by pure CSS

#### Small cookies
Check an object is an array or not:
```javascript
if (Object.prototype.toString.call(arraylist) === '[object Array]') {
  return true;
}

// Or
Array.isArray(arraylist);
```

general guideline for addition operators:
```
Number + Number -> Addition
Boolean + Number -> Addition
Boolean + Boolean -> Addition
Number + String -> Concatenation
String + Boolean -> Concatenation
String + String -> Concatenation
```

Hoisting
```javascript
var salary = "1000$";

(function () {
    console.log("Original salary was " + salary);

    var salary = "5000$";

    console.log("My New Salary " + salary);
})();
// Output will be `undefined, 5000$`
```

CSS Calculating Specificity
```
The actual specificity of a group of nested selectors takes some calculating.
You can give every ID selector a value of 100, every class selector a value of 10
and every HTML selector a value of 1. When you add tehm all up, you have a specificity value.

* p has a specificity of 1
* div p has a specificity of 2
* .tree has a specificity of 10
* div p.tree has specificity of 12
* #baobab has a specificity of 100
* body #content .alternative has a specificity of 112
```

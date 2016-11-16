### front end
#### Resources
* [Cracking the front-end interview](https://medium.freecodecamp.com/cracking-the-front-end-interview-9a34cd46237#.4w3f64tjl)
* [Front-end-Developer-interview-questions](https://github.com/h5bp/Front-end-Developer-Interview-Questions)



#### To Prepare
* Radio button, Check box, Selected
* Form, Figure


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

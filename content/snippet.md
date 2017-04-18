### Dom
#### DocumentFragment
```javascript
let fragment = document.createDocumentFragment(),
  list = ['foo', 'bar', 'baz', ...];

for (var i = 0; i < list.length; i++) {
  const el = document.createElement('li');
  const text = document.createTextNode(list[i]);
  el.appendChild(text);
  fragment.appendChild(el);
}

document.body.appendChild(fragment);
```

### Generator
#### Async with generator
```javascript
function makeAjaxCall(url, cb) {
  // do somw ajax here
  // call cb(result) when complete
}

function request(url) {
  // this is where we're hiding the asynchronicity,
  // away from the main code of our generator
  // it.next() is the generator's iterator-resume call
  makeAjaxCall(url, function(response) {
    it.next(response);
  });
  // Note: nothing returned here!
}

function *main() {
  let result1 = yield request("http://some.url.1");
  let data = JSON.parse(result1);

  let result2 = yield request("http://some.url.2");
  let resp = JSON.parse(result2);
}

let it = main();
it.next(); // get it all started

```
#### Async with generator and promise
```js
function request(url) {
  return new Promise(function(resolve, reject) {
    // pass an error-fist style callback
    makeAjaxCall(url, function(err, text) {
      if (err) reject(err);
      else resolve(text);
    });
  });
}

// run (async) a generator to completion
// Note: simplified approach: no error handling here
function runGenerator(g) {
  let it = g(), ret;

  (function iterate(val) {
    ret = it.next(val);

    if (!ret.done) {
      if ('then' in ret.value) { // Promise
        ret.value.then(iterate);
      } else {
        setTimeout(function() { // immediate value: just send right back in.
          iterate(ret.value);
        }, 0);
      }
    }
  })();
}

function runGenerator(function *main() {
  try {
    let result1 = yield request("http://some.url.1");
  } catch (err) {
    console.log("Error:", err);
    return;
  }
  let data = JSON.parse(result1);

  try {
    let result2 = yield request("http://some.url.2");
  } catch (err) {
    console.log("Error:", err);
    return;
  }
  let resp = JSON.parse(result2);
  console.log("The value you asked for: ", resp.value);
});

```

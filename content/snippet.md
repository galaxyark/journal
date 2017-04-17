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

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

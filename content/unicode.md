# Unicode
Unicode Surrogate Pairs
```javascript
function getSurrogatePair(astralCodePoint) {  
  let highSurrogate =
     Math.floor((astralCodePoint - 0x10000) / 0x400) + 0xD800;
  let lowSurrogate = (astralCodePoint - 0x10000) % 0x400 + 0xDC00;
  return [highSurrogate, lowSurrogate];
}
getSurrogatePair(0x1F600); // => [0xDC00, 0xDFFF]

function getAstralCodePoint(highSurrogate, lowSurrogate) {  
  return (highSurrogate - 0xD800) * 0x400
      + lowSurrogate - 0xDC00 + 0x10000;
}
getAstralCodePoint(0xD83D, 0xDE00); // => 0x1F600  
```

Work around for getting length of astral plane code
```javascript
const str = 'cat\u{1F639}';
console.log(str);  // => cat😹
console.log(str.length); // => 5

const strArr = [...str];
console.log(strArr); // => [ 'c', 'a', 't', '😹' ]
console.log(strArr.length); // => 4
```

Exist 2 possibilities to access astral synmbols correctly in a string:
* Use Unicode-aware string iterator and generate an array of symbols [...str][index]
* Get code point number using `number = myString.codePointAt(index)`, then transform the
number to a symbol using `String.fromCodePoint(number)` - (Recommended option)

```javascript
var omega = '\u{1D6C0} is omega';  
console.log(omega);                        // => '𝛀 is omega'  
// Option 1
console.log([...omega][0]);                // => '𝛀'  
// Option 2
var number = omega.codePointAt(0);  
console.log(number.toString(16));          // => '1d6c0'  
console.log(String.fromCodePoint(number)); // => '𝛀'  
```

# Reference
[What every JavaScript developer should know about Unicode](https://rainsoft.io/what-every-javascript-developer-should-know-about-unicode/)

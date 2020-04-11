## WTFs

```javascript
Number.MAX_VALUE > 0; // true

Number.MIN_VALUE > 0; // true
// Because this is actually the smallest positive number you can get
```

```javascript
1 < 2 < 3; // true
3 > 2 > 1; // false
```

```javascript
42.toFixed(2); // Error
42. toFixed(2); // Error
42 .toFixed(2); // 42.00
42 . toFixed(2); // 42.00
42.0.toFixed(2); // 42.00
42..toFixed(2); // 42.00
```

```javascript
[] == ![]; // true
// This is actually coerced to [] == false, 
// then coerced to 0 == 0, because when anything gets compared to 
// an object, both things end up becoming numbers.

[] + {}; // [object Object] - this is actualy coerced to '' + {}
{} + []; // this is actualy coerced to 0 + 0
```

```javascript
Number(""); // 0
Number(" "); // 0
Number("\r\n\t"); // 0

Number("0"); // 0
Number("-0"); // -0
JSON.parse("-0"); // -0

- 0; // -0
Number("- 0"); // NaN
```

```javascript
JSON.stringify(-0); // "0"
String(-0); // "0"
-0 + ""; // "0"
```

```javascript
Number("0."); // 0
Number(".0"); // 0
Number("."); // NaN

Number(undefined); // NaN
Number(null); // 0

Number("0O0"); // 0
Number("0X0"); // 0
```

```javascript
Number({}); // NaN
Number([]); // 0

String({}); // "[object Object]"
String([]); // ""
```

```javascript
String(null); // "null"
String([null]); // ""

String(undefined); // "undefined"
String([undefined]); // ""
```

```javascript
String([null, null,]); // ","

String([undefined, undefined,]); // ","

String([, ,]); // ","
```

```javascript
var o1 = {foo: "bar"};
var o2 = Object.create(null);
o2.foo = "bar";

o1 + ""; // "[object Object]"
o2 + ""; // TypeError
```

```javascript
// ES6
var s = Symbol("foo");
s; // Symbol(foo)
String(s); // Symbol(foo)
s + ""; TypeError
```

```javascript
Array(3);
// Chrome: [undefined x 3]
// Firefox: [ null, null, null ]

[,,,];
// Chrome: [undefined x 3]
// Firefox: [ null, null, null ]

[undefined, undefined, undefined];
// Chrome: [undefined, undefined, undefined]
// Firefox: [undefined, undefined, undefined]
```

```javascript
var a = [];
a.length = 3;
a;
// Chrome: [undefined x 3]
// Firefox: [ null, null, null ]
```

```javascript
Array.apply(null, Array(3));
// [undefined, undefined, undefined]

Array.apply(null, [,,,]);
// [undefined, undefined, undefined]

Array.apply(null, {length: 3});
// [undefined, undefined, undefined]

Array.apply(null, (a, b, c) => {});
// [undefined, undefined, undefined]
```

```javascript
function foo() {
  try {
    return 2;
  }
  finally {
    return 3;
  }
}

foo(); // 3
```

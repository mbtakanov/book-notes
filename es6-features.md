# EcmaScript 6 features

## Purpose

This repository is created to give a quick overview for anyone who is interested in the new features that ES6 provides. For more detailed information about each feature, please visit [https://developer.mozilla.org/en-US/docs/Web/JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript). In order to stay up to date with the browsers compatibility, visit [https://kangax.github.io/compat-table/es6/](https://kangax.github.io/compat-table/es6/).

## Table of Contents

1. [Variables and Parameters](#variables-and-parameters)
    * [*let* keyword](#let-keyword)
    * [*const* keyword](#const-keyword)
    * [Destructuring assignment](#destructuring-assignment)
    * [Default Parameters](#default-parameters)
    * [Rest parameters](#rest-parameters)
    * [Spread Operator](#spread-operator)
    * [Template Literals](#template-lterals)
2. [Classes](#classes)
    * [*class* keyword](#class-keyword)
    * [constructor](#constructor)
    * [*get* and *set* keywords](#get-and-set-keywords)
    * [*extends* keyword](#extends-keyword)
    * [*super* keyword](#super-keyword)
    * [overrides](#overrides)
3. [Functional JavaScript](#functional-javascript)
    * [Arrow functions](#arrow-functions)
    * [Arrow functions and `this` binding](#arrow-functions-and-this-binding)
    * [Iterables and Iterators](#iterables-and-iterators)
    * [*for of* loop](#for-of-loop)
    * [Generators](#generators)
    * [Comprehension syntax](#comprehension-syntax)
3. [Built-in Objects](#built-in-objects)
    * [Numbers](#numbers)
    * [Math](#math)
    * [Arrays](#arrays)
    * [Sets](#sets)
    * [Maps](#maps)
    * [WeakMap & WeakSet](#weakmap--weakset)
4. [Asynchronous JavaScript](#asynchronous-javascript)
    * [Promises](#promises)
    * [Chaining Promises](#chaining-promises)
    * [Asyncronious generators](#asyncronious-generators)
5. [ES6 Objects](#es6-objects)
    * [Object.is() & Object.assign()](#objectis--objectassign)
    * [Object shorthand and computed properties](#object-shorthand-and-computed-properties)
    * [Proxies](#proxies)
6. [Modules](#modules)
    * [*import* & *export* keywords](#import--export-keywords)
    * [*default* keyword](#default-keyword)

## Variables and Parameters

### *let* keyword

```javascript
function doWork () {
  for (let i = 0; i < 10; i++) {}
  return i;
}

doWork(); // ReferenceError: i is not defined(…)
```

### *const* keyword

```javascript
const MIN_LENGTH = 5;
MIN_LENGTH = 7; // TypeError: Assignment to constant variable.
```

### Destructuring assignment

```javascript
let x = 3;
let y = 5;
[x, y] = [y, x]; // x = 5, y = 3
```

```javascript
let [x, y] = [3, 5];
```

```javascript
function doWork () {
  return [1, 2, 3];
}

let [, x, y, z] = doWork(); // x = 2, y = 3, z = undefined
```

```javascript
function doWork () {
  return {
    firstName: 'test',
    lastName: 'testing',
    misc: {
      facebook: 'testFB'
    }
  };
}

let {
    firstName: first,
    lastName,
    misc: { facebook: fb }
} = doWork(); // first = 'test', lastName = 'testing', fb = 'testFB'
```

```javascript
let doWork = function (config, {data, hasCache}) {
  return data;
}

let foo = doWork(
  'someUrl/test', {
    data: 'test',
    hasCache: false
  }
); //foo = 'test'
```

### Default Parameters

```javascript
let doWork = function (name = 'test') {
  return name;
}

let foo = doWork(''); // foo = ''
let bar = doWork(null); // bar = null
let baz = doWork(undefined); // baz = 'test'
let qux = doWork(); // qux = 'test'
```

```javascript
let doWork = function (config, {data = 'test', hasCache = false}) {
  return data;
}

let foo = doWork(
  'someUrl/test', {
    hasCache: false
  }
); //foo = 'test'
```

### Rest parameters

- The rest paramater is always the last parameter

```javascript
let doWork = function (name, ...numbers) {
  // numbers is empty array by default
  let sum = 0;

  numbers.forEach(function(n) {
    sum += n;
  });

  return sum;
};

let foo = doWork('John', 1, 2, 3, 4, 5); // foo = 15
let bar = doWork('John'); // bar = 0;
```

### Spread Operator

```javascript
let doWork = function (x, y, z) {
  return x + y + z;
}

let foo = doWork(...[1, 2, 3]); // foo = 6
```

```javascript
let foo = [2, 3, 4, 5];
let bar = [1, ...foo, 6, 7, 8, 9, 10]; // bar = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

### Template Literals

- Use \` (backtick) symbol

```javascript
let foo = 'John';
let bar = `Hello, my name is ${foo}.
This is new line.`;
```

- We can call function infront of template literal or a.k.a tagged template literals

```javascript
let doWork = function (string, ...args) {
  // string = ["", " + ", " = ", ". Neat."]
  // args = [1, 2, 3]

  let result = '';

  for (let i = 0; i < string.length; i += 1) {
    result += string[i];
    if (i < args.length) {
      result += args[i];
    }
  }

  return result.toUpperCase();
};

let x = 1;
let y = 2;
let foo = doWork `${x} + ${y} = ${x + y}. Neat.`; // foo = '1 + 2 = 3. NEAT.'
```

## Classes

### *class* keyword

```javascript
class User {
  doWork() {
    return 'test';
  }

  sayHi() {
    return 'Hi';
  }
}

let foo = new User();
foo.doWork(); // test
foo.sayHi(); // Hi
```
- We don't need to use commas and the semi-colons are optional.

### constructor

```javascript
class User {
  constructor(name = 'John Doe') {
    this.name = name;
  }

  sayHi() {
    return `Hi ${this.name}.`; // We are using ` (backtick symbol)
  }
}

let foo = new User('Jane');
foo.sayHi(); // Hi Jane.
```

### *get* and *set* keywords

- Useful to add logic when *get* and *set* are executed.

```javascript
class User {
  constructor(name) {
    this.name = name; // Here we are actualy calling the set function bellow
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (!value || value.length < 3) {
      this._name = 'John Doe';
    } else {
      this._name = value;
    }
  }
}

let foo = new User();
foo.name = '';
foo.name; // John Doe
```

### *extends* keyword

- This keyword allows us to create inheritance between different classes

```javascript
class Animal {
  // ...
}

class Cat extends Animal {
  purr() {
    console.log('Purrr purr purrr');
  }
}

let foo = new Cat();
foo.purr(); // Purrr purr purrr
```

- Animal is the super class for Cat.

### *super* keyword

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name, breed) {
    super(name); // <------- Calling the Animal constructor
    this.breed = breed;
  }
}

let foo = new Cat('Kity', 'Persian cat');
foo.name; // Kity
foo.breed; // Persian cat
```

### Overrides

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return 'Hi.';
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
  }

  sayHi() {
    return super.sayHi() + ' Meow.';
  }

  toString() {
    // We can also override Object functions like .toString(),
    // because Animal actualy extends Object class.
    return 'I\'m cat.';
  }
}

let foo = new Animal('Dogy');
foo.sayHi(); // Hi.

let bar = new Cat('Kity');
bar.sayHi(); // Hi. Meow.
bar.toString(); // I'm cat
```

## Functional JavaScript

### Arrow functions

```javascript
let foo = (a, b) => a * b;
foo(2, 5); // 10
```

```javascript
// We can write multiple lines with arrow functions, 
// but then we must write the return statement
let foo = [1, 2, 3, 4, 5];
let bar = foo.filter(x => {
  return x > 3;
}); // [4, 5]
```

### Arrow functions and `this` binding

- Arrow functions adopt the `this` binding from the enclosing (function or global) scope

- Discard the need to create `var self = this;` variable and use it where the `this` binding is different than the one we need

### Iterables and Iterators

- If an object holds a collection that is `iterable`, we can use its `iterator` and walk through the collection one item at a time

- Every `iterator` has a `.next()` method. That method returns an object every time it's called

- For `.values()`, `.entires()` and `[Symbol.iterator]` methods check for browser compatibility [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

```javascript
let foo = [1, 2, 3];
// let iterator = foo.values(); // One way
// let iterator = foo.entries(); // Another way
let iterator = foo[Symbol.iterator](); // Another way
iterator.next(); // {value: 1, done: false}
iterator.next(); // {value: 2, done: false}
iterator.next(); // {value: 3, done: false}
iterator.next(); // {value: undefined, done: true}
```

- The `iterators` are so called `lazy` and they can do the least amount of work possible. That means that our code might run fast in some scenarios

### *for of* loop

- As we know the `for in` loop works by enumerating the properties of an object. The `for of` loop, on the other hand, will enumerate over the values of an object

```javascript
let foo = ['a', 'b', 'c', 'd'];
for (let n of foo) console.log(n);
// a, b, c, d
```

### Generators

- We can create generators with `*` symbol after the *function* keyword

```javascript
let foo = function*() {
  yield 1;
  yield 2;
  yield 3;
};

let bar = foo();
bar.next(); // { value: 1, done: false }
bar.next(); // { value: 2, done: false }
bar.next(); // { value: 3, done: false }
bar.next(); // { value: undefined, done: true }
```

- One of the big advantages of generators is that we can implement a logic when they stop, this way they won't iterate over the entire collection

```javascript
let foo = function*(items, number) {
  let count = 0;
  if (number < 1) return; // This will set the done flag to true and the iterator will stop

  for (let item of items) {
    yield item;
    count += 1;
    if (count >= number) return;  // This will also set the done flag to true
  } 
};

let bar = [1, 2, 3, 4, 5];
let baz = foo(bar, 2);

for (let n of baz) {
  console.log(n);
}
// 1
// 2
```

- One of the special behavior of generators is that we can pass an argument to the `.next()` method

```javascript
let range = function*(start, end) {
  let current = start;
  while (current <= end) {
    let passedParameter = yield current;
    current += passedParameter || 1;
  }
};

let foo = range(1, 10);


foo.next(); // { value: 1, done: false } - cannot be passed a value to the initial next() method
foo.next(2); // { value: 3, done: false }
foo.next(5); // { value: 8, done: false }
```

### Comprehension syntax

- Array comprehension (uses `[...]`)

```javascript
let foo = [1, 2, 3];
let bar = [for (n of foo) if (n > 1) n * n];
bar; // [4, 9]
```

- Generator comprehension (uses `(...)`)

```javascript
let foo = [1, 2, 3];
let bar = (for (n of foo) if (n > 1) n * n);
Array.from(bar); // [4, 9]
```

## Built-in Objects

### Numbers

- Octal numbers

```javascript
let octal_1 = 071; // 57
let octal_2 = 0o71; // 57
let nOctal = Number("0o71"); // 57
```

- Binary

```javascript
let bin = 0b1101; // 13
let nBin = Number("0b1101"); // 13
```

- Number.parseInt();

- Number.parseFloat();

- Number.isFinite()

Global isFinite() first converts to number (with Number()) and then executes the logic.

Number.isFinite() doesn't convert to number.

- Number.isNaN()

Global isNaN() first converts to number (with Number()) and then executes the logic.

Number.isNaN() doesn't convert to number.

- Number.isInteger()

- Number.MAX_SAFE_INTEGER

JavaScript can deal with numbers correctly in the range of `Number.MAX_SAFE_INTEGER` * -1 to `Number.MAX_SAFE_INTEGER`

- Number.isSafeInteger()

```javascript
Number.isSafeInteger(9007199254740991); // true
Number.isSafeInteger(9007199254740992); // false
```

### Math

- Math.acosh(), Math.asinh(), Math.atanh(), Math.cosh(), Math.sinh(), Math.tanh()

- Math.cbrt(), Math.clz32(), Math.log1p(), Math.log10(), Math.log2(), Math.expm1(), Math.hypot(), Math.fround()

```javascript
Math.log10(100); // 2;
Math.log2(16); // 4;
Math.hypot(3,4); // 5, hypot refer to hypotenuse
Math.fround(2.888); //2.888000011444092
```

- Math.sign(), Math.trunc()

```javascript
Math.sign(572); // 1;
Math.sign(0); // 0;
Math.sign(-83); // -1;
```

```javascript
Math.trunc(2.8); //2 - Truncate the ingeter from a number
Math.trunc(-2.8); // -2;
```

### Arrays

- .find(fn())

```javascript
let foo = [1,2,3];
let bar = foo.find(x => x > 2); // 3
```

- .findIndex(fn())

```javascript
let foo = [1,2,3];
let bar = foo.findIndex(x => x > 2); // 3 - finds the first match and take its index
```

- .fill(item, startIndex, endIndex)

```javascript
let foo = [1,2,3];
let bar = foo.fill('a'); // ['a,'a,'a]
```

- .copyWithin(target, start, end)

```javascript
let foo = [1,2,3,4];
let bar = foo.copyWithin(2, 0); // [1,2,1,2]
let baz = foo.copyWithin(0, -2); // [3,4,3,4]
```

- Array.of()

```javascript
let foo = new Array(3); // [,,]
let bar = Array.of(3); // [3]
```

- Array.from()

```javascript
let foo = document.querySelectorAll('div'); // Doesn't have .forEach
let bar = Array.from(foo); // Have .forEach
```

- .entries()

```javascript
let foo = ['Joe', 'Jack', 'Jim']
let bar = foo.entries();
bar.next(); // {value: [0, 'Joe'], done: false}
bar.next(); // {value: [1, 'Jack'], done: false}
bar.next(); // {value: [2, 'Jim'], done: false}
bar.next(); // {value: undefined, done: true}
```

- .keys()

```javascript
let foo = ['Joe', 'Jack', 'Jim']
let bar = foo.keys();
bar.next(); // {value: 0, done: false}
bar.next(); // {value: 1, done: false}
bar.next(); // {value: 2, done: false}
bar.next(); // {value: undefined, done: true}
```

- Array comperhensions

```javascript
let foo = [for (i of [1,2,3]) i]; // [1,2,3]
let bar = [for (i of [1,2,3]) i*i]; // [1,4,9]
let baz = [for (i of [1,2,3]) if (i < 3) i]; // [1,2]
```

```javascript
let x = [for (first of ['Willian', 'John', 'Blake'])
  for (middle of ['Robert', 'Andrew', 'John'])
  if (first !== middle) (first + ' ' + middle + 'Smith')]
// x = ["Willian RobertSmith", "Willian AndrewSmith", 
    "Willian JohnSmith", "John RobertSmith", 
    "John AndrewSmith", "Blake RobertSmith", 
    "Blake AndrewSmith", "Blake JohnSmith"]
```

### Sets

- Set objects are collections of values. You can iterate through the elements of a set in insertion order. A value in the Set may only occur once; it is unique in the Set's collection.

- .size, .add(), .has(), .clear(), .delete(), .forEach(), .entries(), .values()

```javascript
let set = new Set();
set.size; // 0

set.add(1);
set.size; // 1

let objectValue = {};
set.add(objectValue);
set.delete(objectValue);

set.clear();

set.add('a');
let e = set.entries();
e.next(); // {value: [0, 'a'], done: false}
e.next(); // {value: undefined, done: true}

let s2 = new Set(set.values()); // Set {'a'}
let s3 = new Set(s2); // Set {'a'}
```

### Maps

- The Map object holds key-value pairs and remembers the original insertion order of the keys. Any value (both objects and primitive values) may be used as either a key or a value.

- .size, .set(), .get(), .has(), .clear(), .delete(), .entries(), .values(), .keys(), .forEach(function(value, key) {})

```javascript
let map = new Map();
map.set("age", 28);
map.get("age"); // 28

let foo = {id: 1};
map.set(foo, 42);
map.get(foo); // 42
```

```javascript
let map = new Map([['name', 'John'], ['age', 28], ['weight', 70]]);
```

- If duplicate key is added, it will replace the old value

```javascript
let map = new Map();
let key = {};
map.set(key, 1);
map.set(key, 2);
map.get(key); // 2
```

```javascript
let map = new Map([['name', 'John'], ['age', 28], ['weight', 70]]);
```

```javascript
for (let item of map) {
  // item is an array like ['name', 'John']
}
```

```javascript
for (let [key, value] of map) {
  // use key and value letiables
}
```

### WeakMap & WeakSet

The pointers in the WeakMap & WeakSet are not strong pointers, therefore
their values can be removed from the garbage collector (if the values are not in use anywhere else).

WeakSet don't have: .size, .entries, .values and .forEach, for (let key of myWeakSet)
WeakSet have: .has(), .delete(), .clear()

WeakMap don't have: .size, .entries, .keys, .values and .forEach, for (let key of myWeakMap)
WeakMap have: .has(), .get(), .delete(), .clear()


## Asynchronous JavaScript

### Promises

```javascript
let promise = new Promise(function(resolve, reject) {
  resolve('foo');
});

promise.then(function(data) {
  // success
  console.log(data); // foo
}, function(error) {
  // reject
  console.log(error); // bar
});
```

```javascript
let firstPromise = new Promise(function(resolve, reject) {
  resolve('foo');
});

let secondPromise = Promise.resolve(function(resolve, reject) {
  resolve(firstPromise);
});

// let secondPromise = Promise.resolve(firstPromise);
// let secondPromise = Promise.reject(firstPromise);

secondPromise.then(function(data) {
  // success
  console.log(data); // foo
}, function(error) {
  // reject
  console.log(error); // bar
});
```

### Chaining Promises

```javascript
function getDataA (id) {
  return Promise.resolve({id: id});
}

function getDataB (name) {
  return Promise.resolve({name: name});
}

function getDataC (age) {
  return Promise.resolve({age: age});
}

getDataA(42).then((data) => {
  console.log(data.id);
  return getDataB('foo');
})
.then((data) => {
  console.log(data.name);
  return getDataC(25);
})
.then((data) => {
  console.log(data.age);
})
.catch((error) => {
  console.log(error);
});
```

```javascript
// Wait all promises to be resolved
let numbers = [1, 2, 3];
let queue = [];

function getDataA (id) {
  return Promise.resolve({id: id});
}

for (let i = 0; i < numbers.length; i++) {
  queue.push(getDataA(numbers[i]));
}

Promise.all(queue).then((data) => {
  console.log(data); // [{id: 1}, {id: 2}, {id: 3}]
});
```

```javascript
// Resolve after the first promise
let numbers = [1, 2, 3];
let queue = [];

function getDataA (id) {
  return Promise.resolve({id: id});
}

for (let i = 0; i < numbers.length; i++) {
  queue.push(getDataA(numbers[i]));
}

Promise.race(queue).then((data) => {
  console.log(data);
  // {id: ???} - we dont' know the id because the order is unknown
});
```

### Asyncronious generators

TODO:

## ES6 Objects

### Object.is() & Object.assign()

The [Object.is()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) method determines whether two values are the same value. (===)

Edge cases:
```javascript
-0 === 0; // true
Object.is(-0, 0); // false

NaN === NaN; // false
Object.is(NaN, NaN); // true
```

- The [Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) method is used to copy the values of all enumerable own properties from one or more source objects to a target object. It will return the target object.

```javascript
var foo = {a: 1};
var bar = Object.assign({}, foo);
console.log(bar); // {a: 1}
```

For deep cloning, we need to use other alternatives because Object.assign() copies property values. If the source value is a reference to an object, it only copies that reference value - JSON.parse(JSON.stringify());

Merging objects:
```javascript
var foo = {a: 1};
var bar = {b: 2};
var baz = {c: 3};

Object.assign(foo, bar, baz);
console.log(foo);  // {a: 1, b: 2, c: 3}
```

Merging objects with same properties - the properties are overwritten by other objects that have the same properties later in the parameters order.
```javascript
var foo = {a: 1, b: 1, c: 1};
var bar = {b: 2, c: 2};
var baz = {c: 3};

var xyz = Object.assign({}, foo, bar, baz);
console.log(xyz); // {a: 1, b: 2, c: 3}
```

### Object shorthand and computed properties

- Object shorthand

```javascript
let model = 'Ford';
let year = 1969;

let classic = {
  model: model,
  year: year
};

let modern = {
  model,
  year
};

console.log(classic);
console.log(modern);
```

- Computed property names

```javascript
// EC5
function createSimpleObject (propName, propVal) {
  let foo = {};
  foo[propName] = propVal;
  return foo;
}
```

```javascript
// EC6
function createSimpleObject (propName, propVal) {
  return {
    [propName]: propVal
  };
}
```
The brackets *[...]* mean that the value inside is expression and should be evaluated.


### [Proxies](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

The Proxy object is used to define custom behavior for fundamental operations (e.g. property lookup, assignment, enumeration, function invocation, etc).

```javascript
// Intercepting get and set requests
let foo = {
  age: 28,
  name: 'Foo'
};

let handler = {
  get: function(target, prop) {
    if (prop === 'age') {
      return 'Awesome ' + target[prop];
    } else {
      return target[prop];
    }
  },
  set: function(target, prop, value) {
    if (prop === 'name' && value.length < target[prop].length) {
      console.log('Name must be longer than the current value.');
    } else {
      target[prop] = value;
    }
  },
  delete: ...,
  define: ...,
  freeze: ...,
  seal: ...,
  hasOwnProperty: ...
};

let fooProxy = new Proxy(foo, handler);

fooProxy.age; // "Awesome 28"
fooProxy.name = 'F'; // Name must be longer than the current value.
fooProxy.name; // Foo
```


- Proxying functions

```javascript
let foo = {
  age: 28,
  name: 'Foo',
  attack: function(target) {
    return target.name + ' was obliterated!';
  }
};

foo.attack = new Proxy(foo.attack, {
  // The original foo.attack is no longer accessible
  apply: function(target, context, args) {
    // If the context is not foo, we'll return a warning that noone except foo can use the attack function.
    if (context !== foo) {
      return 'The attack function can be used only by foo.';
    } else {
      return target.apply(context, args);
    }
  }
});

let bar = {name: 'Bar'};
bar.attack = foo.attack;
bar.attack(foo); // The attack function can be used only by foo.
foo.attack(bar); // Bar was obliterated!
```

## Modules

### [*import*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) & [*export*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) keywords

Currently (January, 2017), no browser supports the *import* and *export* keywords.

- Import/Export one by one

```javascript
// foo.js
export class Foo {
  doWork {
    return 'Foo is working.';
  }
}

export let goThere = function() {
  console.log('Go there.'); 
};

export let MAX_LENGTH = 5;


// bar.js
import {Foo, goThere, MAX_LENGTH} from './foo.js';
let f = new Foo();
f.doWork(); // Foo is working.
goThere(); // Go there.
console.log(MAX_LENGTH); // 5
```

- Import the whole module

```javascript
// foo.js
export class Foo {
  doWork {
    return 'Foo is working.';
  }
}

export let goThere = function() {
  console.log('Go there.'); 
};

export let MAX_LENGTH = 5;


// bar.js
module foo from './foo.js';
let f = foo.Foo();
f.doWork(); // Foo is working.
foo.goThere(); // Go there.
console.log(foo.MAX_LENGTH); // 5
```

### *default* keyword

Named exports are useful to export several values. During the import, one will be able to use the same name to refer to the corresponding value.
Concerning the default export, there is only a single default export per module. A default export can be a function, a class, an object or anything else. This value is to be considered as the "main" exported value since it will be the simplest to import.

```javascript
// foo.js
export default class Foo {
  doWork {
    return 'Foo is working.';
  }
}


// bar.js
import customFooName from './foo.js';
let f = new customFooName();
f.doWork(); // Foo is working.
```

### Hiding details

```javascript
// foo.js
export default class Foo {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  doWork {
    return 'Foo is working.';
  }
}


// bar.js
import Foo from './foo.js';
let f = new Foo('Foo');
f._name = 'Bar';
f.doWork(); // Bar is working
```

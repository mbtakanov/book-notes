### Chapter 1: *this* Or That?

*TODO*

### Chapter 2: *this* All Makes Sense Now!

- When a function is invoked with `new` in front of it, otherwise known as a constructor call, the following things are done automatically:
  1. a brand new object is created (aka, constructed) out of thin air
  2. *the newly constructed object is `[[Prototype]]`-linked*
  3. the newly constructed object is set as the `this` binding for that function call
  4. unless the function returns its own alternate **object**, the `new`-invoked function call will *automatically* return the newly constructed object.

### Chapter 3: Objects

- Define properties

```javascript
var myObject = {};
Object.defineProperty( myObject, "a", {
    value: 2,
    writable: true,
    configurable: true,
    enumerable: true
});

myObject.a; // 2

myObject.seal(); // myObject.preventExtensions(..) + configurable:false

myObject.freeze(); // myObject.seal(); + writable:false
```
- Getters and setters

```javascript
var myObject = {
    // define a getter for a
    get a() {
        return this._a_;
    },

    // define a setter for a
    set a(val) {
        this._a_ = val * 2;
    }
};
```

- How to check if there is property with value `undefined` or there is no property at all?

```javascript
var myObject = {
    a: 2
};

("a" in myObject);              // true
("b" in myObject);              // false
myObject.hasOwnProperty( "a" ); // true
myObject.hasOwnProperty( "b" ); // false
```
- Check property is enumerable

```javascript
var myObject = {};

Object.defineProperty(
    myObject,
    "a",
    // make a enumerable, as normal
    { enumerable: true, value: 2 }
);

Object.defineProperty(
    myObject,
    "b",
    // make b non-enumerable
    { enumerable: false, value: 3 }
);

myObject.propertyIsEnumerable( "a" ); // true
myObject.propertyIsEnumerable( "b" ); // false
Object.keys( myObject ); // ["a"]
Object.getOwnPropertyNames( myObject ); // ["a", "b"]
```
- Symbol.iterator

```javascript
var myArray = [ 1, 2, 3 ];
var it = myArray[Symbol.iterator]();
it.next(); // { value:1, done:false }
it.next(); // { value:2, done:false }
it.next(); // { value:3, done:false }
it.next(); // { done:true }
```

- Many people mistakenly claim "everything in JavaScript is an object", but this is incorrect.

### Chapter 4: Mixing (Up) "Class" Objects

- Classes are an optional pattern in software design, and you have the choice to use them in JavaScript or not.

- A class is instantiated into object form by a copy operation.

- Class inheritance implies copies.

- No multiple inheritance in JavaScript

- Parasitic inheritance (the prototype thing)

### Chapter 5: Prototypes

- The default `[[Get]]` operation proceeds to follow the `[[Prototype]]` link of the object if it cannot find the requested property on the object directly.

- Shadowing explained

- JavaScript is almost unique among languages as perhaps the only language with the right to use the label "object oriented", because it's one of a very short list of languages where an object can be created directly, without a class at all.

```javascript
function Foo() { /* ... */ }

var a = new Foo();

Object.getPrototypeOf( a ) === Foo.prototype; // true
```

- **We end up with two objects, linked to each other.** That's *it*. We didn't instantiate a class. We certainly didn't do any copying of behavior from a "class" into a concrete object. We just caused two objects to be linked to each other.

- "(Prototypal) Inheritance" explained

```javascript
// pre-ES6
// throws away default existing `Bar.prototype`
Bar.prototype = Object.create( Foo.prototype );

// ES6+
// modifies existing `Bar.prototype`
Object.setPrototypeOf( Bar.prototype, Foo.prototype );
```

-----------

```javascript
function isRelatedTo(o1, o2) {
    function F(){}
    F.prototype = o2;
    return o1 instanceof F;
}

var a = {};
var b = Object.create( a );

isRelatedTo( b, a ); // true
```

- `Foo.prototype.isPrototypeOf(a);` checks does in the entire [[Prototype]] chain of `a`, `Foo.prototype` ever appear?

- `Object.getPrototypeOf(a) === Foo.prototype; // true`

- Most browsers (not all!) have also long supported a non-standard alternate way of accessing the internal `[[Prototype]]`:

```javascript
a.__proto__ === Foo.prototype;  // true
```

- The JavaScript community unofficially coined a term for the double-underscore, specifically the leading one in properties like `__proto__`: "dunder". So, the "cool kids" in JavaScript would generally pronounce `__proto__` as "dunder proto".

### Chapter 6: Behavior Delegation


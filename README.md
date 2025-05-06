# JavaScript Style Guide

## A. Parens, Braces, Linebreaks

Use whitespace and braces to promote readability.

```js
// Bad:
if(condition) doSomething();
while(condition) iterating++;
for(var i=0;i<100;i++) someIterativeFn();

// Good:
if ( condition ) {
  // statements
}

while ( condition ) {
  // statements
}

for ( var i = 0; i < 100; i++ ) {
  // statements
}

// Even better:
var i, length = 100;

for ( i = 0; i < length; i++ ) {
  // statements
}

var i = 0, length = 100;
for ( ; i < length; i++ ) {
  // statements
}

var prop;
for ( prop in object ) {
  // statements
}

if ( true ) {
  // statements
} else {
  // statements
}
```

## B. Assignments, Declarations, Functions

### Variables

```js
// Bad
var foo = "", bar = "";
var qux;

// Good
var foo = "";
var bar = "";
var qux;

// or
var foo = "",
    bar = "",
    qux;

// or with comments
var // Comment on these
    foo = "",
    bar = "",
    quux;
```

### Variable Declarations Should Be At The Top

```js
// Bad
function foo() {
  // some statements here
  var bar = "",
      qux;
}

// Good
function foo() {
  var bar = "",
      qux;
  // all statements after the variables declarations
}
```

### Use const/let at the top of the scope (ES6)

```js
// Bad
function foo() {
  let foo, bar;
  if ( condition ) {
    bar = "";
  }
}

// Good
function foo() {
  let foo;
  if ( condition ) {
    let bar = "";
  }
}
```

### Functions

```js
// Named Function Declaration
function foo(arg1, argN) {}
foo(arg1, argN);

function square(number) {
  return number * number;
}
square(10);

function square(number, callback) {
  callback(number * number);
}
square(10, function(square) {
  // callback statements
});

// Function Expression
var square = function(number) {
  return number * number;
};

// Named Function Expression
var factorial = function factorial(number) {
  if (number < 2) return 1;
  return number * factorial(number - 1);
};

// Constructor
function FooBar(options) {
  this.options = options;
}
var fooBar = new FooBar({ a: "alpha" });
fooBar.options; // { a: "alpha" }
```

## C. Exceptions, Slight Deviations

```js
foo(function() {
  // no extra space
});

foo(["alpha", "beta"]);

foo({
  a: "alpha",
  b: "beta"
});

foo("bar");

if (!("foo" in obj)) {
  obj = (obj.bar || defaults).baz;
}
```

## D. Consistency Always Wins

Choose one style and be consistent.

```js
if (condition) {
  // statements
}

while (condition) {
  // statements
}

for (var i = 0; i < 100; i++) {
  // statements
}

if (true) {
  // statements
} else {
  // statements
}
```

## E. Quotes

Pick single or double quotes and stick to one style across the project.

## F. End of Lines and Empty Lines

Avoid trailing whitespace and unnecessary empty lines.

---

## Type Checking

### Actual Types

```js
typeof variable === "string"
typeof variable === "number"
typeof variable === "boolean"
typeof variable === "object"
Array.isArray(arrayLikeObject)
elem.nodeType === 1
variable === null
variable == null
typeof variable === "undefined"
object.prop === undefined
object.hasOwnProperty("prop")
"prop" in object
```

### Coerced Types

```js
var foo = 0;
foo = document.getElementById("foo-input").value; // string
foo = +document.getElementById("foo-input").value; // number
```

Common Coercions:

```js
var number = 1,
    string = "1",
    bool = false;

number + ""; // "1"
+string; // 1
+string++; // 1
+bool; // 0
bool + ""; // "false"
```

Strict vs Loose Comparison:

```js
"1" === 1; // false
"1" == 1; // true
```

Truthy/Falsy:

```js
// Truthy:
"foo", 1

// Falsy:
"", 0, null, undefined, NaN
```

---

## Conditional Evaluation

```js
if (array.length) {}
if (!array.length) {}
if (string) {}
if (!string) {}
if (foo) {}
if (!foo) {}
if (foo == null) {}
```

---

## Practical Style

### Module Pattern

```js
(function(global) {
  var Module = (function() {
    var data = "secret";
    return {
      bool: true,
      string: "a string",
      array: [1, 2, 3, 4],
      object: { lang: "en-US" },
      getData: function() { return data; },
      setData: function(value) { return (data = value); }
    };
  })();
  global.Module = Module;
})(this);
```

### Constructor

```js
(function(global) {
  function Ctor(foo) {
    this.foo = foo;
    return this;
  }
  Ctor.prototype.getFoo = function() { return this.foo; };
  Ctor.prototype.setFoo = function(val) { return (this.foo = val); };
  global.ctor = function(foo) { return new Ctor(foo); };
})(this);
```

---

## Naming

Use meaningful names. Avoid one-letter or overly terse names.

```js
function query(selector) {
  return document.querySelectorAll(selector);
}

var idx = 0,
    elements = [],
    matches = query("#foo"),
    length = matches.length;

for (; idx < length; idx++) {
  elements.push(matches[idx]);
}
```

### Naming Conventions

```js
// Strings: `dog`
// Arrays: `dogs`
// camelCase for vars/functions
// PascalCase for constructors
// CONSTANT_CASE for symbolic constants
```

---

## Faces of `this`

Prefer `.bind(this)` over aliasing `var self = this`

```js
function Device(opts) {
  this.value = null;
  stream.read(opts.path, function(data) {
    this.value = data;
  }.bind(this));

  setInterval(function() {
    this.emit("event");
  }.bind(this), opts.freq || 100);
}
```

Libraries:

```js
// lodash/underscore
_.bind(function() {}, this);

// jQuery
$.proxy(function() {}, this);

// dojo
dojo.hitch(this, function() {});
```

As last resort:

```js
var self = this;
function() { self.value = data; };
```

---

## Use thisArg

```js
var obj = { f: "foo", b: "bar", q: "qux" };
Object.keys(obj).forEach(function(key) {
  console.log(this[key]);
}, obj);
```

---

## Avoid switch (if possible)

```js
switch(foo) {
  case "alpha": alpha(); break;
  case "beta": beta(); break;
  default: break;
}

// Better alternative:
var cases = {
  alpha: function() { return ["Alpha", arguments.length]; },
  beta: function() { return ["Beta", arguments.length]; },
  _default: function() { return ["Default", arguments.length]; }
};

function delegator() {
  var args = [].slice.call(arguments);
  var key = args.shift();
  var delegate = cases.hasOwnProperty(key) ? cases[key] : cases._default;
  return delegate.apply(null, args);
}

// Usage
delegator("alpha", 1, 2, 3);
```

---



## 7. Code Clarity and Flow Control

### A. Conditional Delegation Example

Of course, the `caseKey` argument could easily be based on some other arbitrary condition.

```js
var caseKey, someUserInput;

// Possibly some kind of form input?
someUserInput = 9;

if (someUserInput > 10) {
  caseKey = "alpha";
} else {
  caseKey = "beta";
}

// or...

caseKey = someUserInput > 10 ? "alpha" : "beta";

// And then...

delegator(caseKey, someUserInput);
// [ "Beta", 1 ]

// And of course...

delegator();
// [ "Default", 0 ]
```

### B. Early Returns Promote Code Readability

Using early returns can simplify function flow and make the code easier to read, with negligible performance difference.

#### 7.B.1.1

**Bad:**

```js
function returnLate(foo) {
  var ret;

  if (foo) {
    ret = "foo";
  } else {
    ret = "quux";
  }
  return ret;
}
```

**Good:**

```js
function returnEarly(foo) {
  if (foo) {
    return "foo";
  }
  return "quux";
}
```


Stay consistent, name thoughtfully, and write clear, maintainable code!

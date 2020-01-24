---
title: JavaScript
layout: language
---

> **Note**: some of this is not JavaScript itself, but [Web API](https://developer.mozilla.org/en/docs/Web/API)

# Poor Design

* Every script is executed in a single global namespace that is accessible in browsers with the window object.
* Camel case sucks:

```
XMLHttpRequest
HTMLHRElement
```

* Automatic type conversion between strings and numbers, combined with `+` overloaded to mean concatenation and addition.
This creates very counterintuitive effects if you accidentally convert a number to a string:

```javascript
var i = 1;
// some code
i = i + ""; // oops!
// some more code
i + 1;  // evaluates to the String '11'
i - 1;  // evaluates to the Number 0
```

* The automatic type conversion of the + function also leads to the intuitive effect that += 1 is different than the ++ operator.
This also takes place when sorting an array:

```javascript
var j = "1";
j++; // j becomes 2

var k = "1";
k += 1; // k becomes "11"

[1,5,20,10].sort() // [1, 10, 20, 5]
```

* The `var` statement uses function scope rather than block scope, which is a completely unintuitive behavior. You may want to use let instead.

# Type System

* JavaScript puts the world into a neat prototype hierarchy with Object at the top. In reality values do not fit into a neat hierarchy.
* You can't inherit from Array or other builtin objects. The syntax for prototypal inheritance also tends to be very cryptic and unusual. (Fixed in ES6)
* In JavaScript, prototype-based inheritance sucks: functions set in the prototype cannot access arguments and local variables in the constructor,
which means that those "public methods" cannot access "private fields".
There is little or no reason for a function to be a method if it can't access private fields. (Fixed in ES6 via symbols)
* JavaScript doesn't support hashes or dictionaries. You can treat objects like them, however, Objects inherit properties from `__proto__` which causes problems.
(Use Object.create(null) in ES5, or Map in ES6)
* Arguments is not an Array. You can convert it to one with slice (or Array.from in ES6):

```javascript
var args = Array.prototype.slice.call(arguments);
// arguments will be deprecated eventually
```

* The number type has precision problems.

```javascript
0.1 + 0.2 === 0.30000000000000004;
```

The problem is not the result, which is expected, but the choice of using floating point number to represent numbers,
and this is a lazy language designer choice. See [http://www.math.umd.edu/~jkolesar/mait613/floating_point_math.pdf](http://www.math.umd.edu/~jkolesar/mait613/floating_point_math.pdf).

* NaN stands for not a number, yet it is a number.

```javascript
typeof NaN === "number"

// To make matters worse NaN doesn't equal itself
NaN != NaN
NaN !== NaN

// This checks if x is NaN
x !== x
// This is the proper way to test
isNaN(x)
```
This is as it should be, as per IEEE754.  Again, the problem is the indiscriminate choice of IEEE754 by the language designer or implementer.

* null is not an instance of Object, yet `typeof null === 'object'`

# Bad Features

> Note: You can bypass many of these bad features by using linters

* JavaScript inherits many bad features from C, including switch fallthrough, and the position sensitive ++ and -- operators.
* JavaScript inherits a cryptic and problematic regular expression syntax from Perl.
* Keyword 'this' is ambiguous, confusing and misleading

```javascript
// This as a local object reference in a method
object.property = function foo() {
    return this; // This is the object that the function (method) is being attached to
}

// This as a global object
var functionVariable = function foo() {
    return this; // This is a Window
}

// This as a new object
function ExampleObject() {
    this.someNewProperty = bar; // This points to the new object
    this.confusing = true;
}

// This as a locally changing reference

function doSomething(somethingHandler, args) {
    somethingHandler.apply(this, args); // Here this will be what we 'normally' expect
    this.foo = bar; // This has been changed by the call to 'apply'
    var that = this;

    // But it gets better, because the meaning of this can change three times in a single function
    someVar.onEvent = function () {
        that.confusing = true;
        // Here, this would refer to someVar
    }
}
```

* Semi colon insertion

```javascript
// This returns undefined
return
{
    a: 5
};
```

* Objects, and statements and labels have very similar syntax.
The example above was actually returning undefined, then forming a statement.
This example actualy throws a syntax error.

```javascript
// This returns undefined
return
{
    'a': 5
};
```

* Implied globals:

```javascript
function bar() {
    // Oops I left off the var keyword, now I have a global variable
    foo = 5;
}
```
_This can be fixed by using 'use strict' in ES5._

* The `==` operator allows type coercion, which is a useful feature, but not one you would want by default.
The operator `===` fixes the problem by not type coercing, but is confusing coming from other languages.

```javascript
0 == ""
0 == "0"
0 == " \t\r\n "
"0" == false
null == undefined

""    != "0"
false != "false"
false != undefined
false != null
```

* Typed wrappers suck:

```javascript
new Function("x", "y", "return x + y");
new Array(1, 2, 3, 4, 5);
new Object({"a": 5});
new Boolean(true);
```

* `parseInt` has a really weird default behavior so you are generally forced into adding that you want your radix to be 10:

```javascript
parseInt("72", 10);
```
_You can use `Number('72')` to convert to a number._

* The (deprecated) with statement sucks because it is error-prone.

```javascript
with (obj) {
    foo = 1;
    bar = 2;
}
```

* The for in statement loops through members inherited through the prototype chain,
so you generally have to wrap it in a long call to `object.hasOwnProperty(name)`, or use `Object.keys(...).forEach(...)`

```javascript
for (var name in object) {
    if (object.hasOwnProperty(name)) {
        /* ... */
    }
}
// Or
Object.keys(object).forEach(function() { ... });
```

* There aren't numeric arrays, only objects with properties, and those properties are named with text strings;
as a consequence, the for-in loop sucks when done on pseudo-numeric arrays because the iterating variable is actually a string,
not a number (this makes integer additions difficult as you have to manually `parseInt` the iterating variable at each iteration).

```javascript
var n = 0;
for (var i in [3, 'hello world', false, 1.5]) {
    i = parseInt(i); // output is wrong without this cumbersome line
    alert(i + n);
}
// Or
[3, 'hello world', false, 1.5].map(Number).forEach(function() { alert(i + n) });
```

* There are also many [deprecated features](https://developer.mozilla.org/en/JavaScript/Reference/Deprecated_Features) such as getYear and setYear on Date objects.

# Missing Features

* It has taken till ES6 to enforce immutability. This statement doesn't work for JavaScript's most important datatype: objects, and you have to use `Object.freeze(...)`.

```javascript
// It works okay for numbers and strings
const pi = 3.14159265358;
const msg = "Hello World";

// It doesn't work for objects
const bar = {"a": 5, "b": 6};
const foo = [1, 2, 3, 4, 5];

// You also can't easily make your parameters constant
const func = function() {
    const x = arguments[0], y = arguments[1];

    return x + y;
};
```

* There should be a more convenient means of writing functions that includes implicit return,
especially when you are using functional programming features such as map, filter, and reduce. (ES6 fixed this)

```javascript
// ES6
x -> x * x
```

* Considering the importance of exponentiation in mathematics, `Math.pow` should really be an infix operator such as `**` rather than a function. (Fixed in ES6 with `**`)

```javascript
Math.pow(7, 2); // 49
```

* The standard library is non-existent. This results in browsers downloading hundreds of KBs of code per page-hit of every webpage
in the world just to be able to do things we usually take for granted.

# DOM

* Browser incompatibilities between Firefox, Internet Explorer, Opera, Google Chrome, Safari, Konqueror, etc make dealing with the DOM a pain.
* If you have an event handler that calls `alert()`, it always cancels the event, regardless of whether you want to cancel the event or not

```javascript
// This event handler lets the event propagate
function doNothingWithEvent(event) {
    return true;
}

// This event handler cancels propagation
function doNothingWithEvent(event) {
    alert('screwing everything up');
    return true;
}
```

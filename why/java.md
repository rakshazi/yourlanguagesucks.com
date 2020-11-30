---
title: Java
layout: language
date: 2017
author: theory.org
---
# Syntax

* Overly verbose.
* Java has unused keywords, such as `goto` and `const`.
* There's no operator overloading... except for strings.
So for pseudo-numeric classes, like BigInteger, you have to do things like `a.add(b.multiply(c))`, which is really ugly.
* There are no delegates; everytime you need a function pointer you have to implement a factory design.
* Arrays don't work with generics: you can't create an array of a variable type `new T[42]`, boxing of array is required to do so:
`class GenSet<E> { Object[] a; E get(int i){return a[i];}}`
* No properties.  Simple class definitions are 7 to 10 times as long as necessary.
* No array or map literals. Array and Map are collection interfaces.
* Before Java 10, no `var` keyword existed for infering local types (like in C#). Please mind that it is antidesign example and class names should never be that long. Example:

```java
// In Java
ClassWithReallyLongNameAndTypeParameter<NamingContextExtPackage> foo = new ClassWithReallyLongNameAndTypeParameter<>();
// In C# | Could easily be just:
var foo = new ClassWithReallyLongNameAndTypeParameter<NamingContextExtPackage>();
// In Java | Same goes for function calls:
SomeTypeIHaveToLookUpFirstButIActuallyDontReallyCareAboutIt result = getTransactionResult();
```

* It is impossible to write Java without an IDE with autocompletion, code generation, import management and refactoring.
* No Tuples. Returning two values from a function or placing a pair in a set warrants a new class in a new file.
Generic `Pair` class leads to hairy types all over the place.

# Model

* No first-class functions and classes.
* Checked exceptions are an experiment [that failed](https://www.artima.com/intv/handcuffs.html).
* There's `int` types and `Integer` objects, `float` types and `Float` objects. So you can have efficient data types, or object-oriented data types.
* * Number base-class doesn't define arithmetic operations, so it is impossible to write useful generics for Number subclasses.
* Before Java 8, using only interfaces to implement multiple inheritance didn't allow sharing common code through them.

# Library

* Functions in the standard library do not use consistent naming,
acronym and capitalization conventions, making it hard to remember exactly what the items are called:
* * This is actually part of the standard, you write all of them in uppercase if it has 3 or less letters, and only the first if it has more so,
`RPGGame` and not `RpgGame`, and `TLSConnection` but you should use `Starttls`.
* The design behind `Cloneable` and `clone` [is just broken](http://www.artima.com/intv/bloch13.html).
* Arrays are objects but don't properly implement `.toString()` (if you try to print an array, it just prints hash code gibberish)
or `.equals()` (arrays of the same contents don't compare equal, which causes a headache if you try to put arrays into collections)
* Before Java 8, useful methods like sorting, binary search, etc. were not part of Collection classes, but part of separate "utility classes" like `Collections` and `Arrays`
* Why is `Stack` a class, but `Queue` an interface?
* * `Stack` belongs to old collection API and should not be used anymore. Use `Deque`(interface) and `ArrayDeque`(implementation) instead.
* Code is cluttered with type conversions.  Arrays to lists, lists to arrays, java.util.Date to java.sql.Date, etc.
* The Date API is considered deprecated, but still used everywhere. The replacement Calendar is not.
* Before Java 8 there was no string join function.
* The reflection API requires multiple lines of code for the simplest operations.
* `(a|[^d])` regex throws StackOverflowException on long strings
* No unsigned numeric types

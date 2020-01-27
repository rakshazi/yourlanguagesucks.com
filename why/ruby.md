---
title: Ruby
layout: language
date: 2017
author: theory.org
---
* `String#downcase`? Who calls it "downcase?" It's called "lower case," and the method should be called "lowercase" or "lower".
And `String#upcase` should have been called "uppercase" or "upper". It does not actually make Ruby suck, it's just a matter of personal taste.
* Unicode support should have been built in from 1.0, not added after much complaining in 1.9/2.0 in 2007
* No support for negative / positive look-behind in regular expressions in 1.8
* Regular expressions are always in multi-line mode
* Using `@` and `@@` to access instance and class members can be unclear at a glance.
* There are no smart and carefully planned changes that can't break compatibility; even minor releases can break compatibility:
[See "Compatibility issues" and "fileutils"](http://svn.ruby-lang.org/repos/ruby/tags/v1_8_7/NEWS).
This leads to multiple recommended stable versions: [both 1.8.7 and 1.9.1 for Windows](http://www.ruby-lang.org/en/downloads/). Which one to use?
* Experimental and (known to be) buggy features are added to the production and "stable" releases:
[See "passing a block to a Proc"](http://svn.ruby-lang.org/repos/ruby/tags/v1_8_7/NEWS).
* There's some minor gotchas. `nil.to_i` turns nil into 0, but 0 does not evaluate as nil. `nil.to_i.nil? #=> false`
* `String#to_i` just ignores trailing characters, meaning: `"x".to_i == 0`
* Aliased methods in the standard library make reading code written by others more confusing,
if one is not yet well familiar with basic stuff. E.g. `Array#size`/`Array#length`, `Array#[]`/`Array#slice`
* Mutable types like arrays are still hashable. This can cause a hash to contain the same key twice, and return a random value (the first?) when accessing the key.
* Omitting parenthesis in function calls enable you to implement/simulate property setter, but can lead to ambiguities.
* Minor ambiguities between the hash syntax and blocks (closures), when using curly braces for both.
* Suffix-conditions after whole blocks of code, e.g. `begin ... rescue ... end if expr`. You are guaranteed to miss the `if expr` if there are a lot of lines in the code block.
* The `unless` keyword (acts like `if not`) tends to make the code harder to comprehend instead of easier, for some people.
* Difference between unqualified method calls and access of local variables is not obvious.
This is especially bad in a language that does not require you to declare local variables and where you can access them without error before you first assign them.
* "Value-leaking" functions. The value of the last expression in an function is implicitly the return value.
You need to explicitly write `nil` if you want to make your function "void" (a procedure). Like if you really care about that return value.
* _``_ syntax with string interpolation for running sub processes. This makes shell injection attacks easy.
* Regular expressions magically assign variables: `$1`, `$2`, ...
* Standard containers (`Array`, `Hash`) have a very big interfaces that make them hard to emulate.
So don't emulate - inherit instead. `class ThingLikeArray < Array; end`
* Symbols and strings are both allowed and often used as keys in hashes, but `"foo" != :foo`,
which led to inventions like `HashWithIndifferentAccess`.
* Parser errors could be more clear. "syntax error, unexpected kEND, expecting $end"
actually means "syntax error, unexpected keyword 'end', expecting end of input"
* Symbols are unequal if their encoded bytes are unequal even if they represent the same strings.

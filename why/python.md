---
title: Python
layout: language
date: 2020
draft: true
author: theory.org
missing:
- correct page structure
- perfomance section
- types section
- missing features section
- toolchain (django, flask, pip, pandas, etc.)
- references to related benchmarks, libraries, PEPs, etc.
- community section
---
* mandatory self argument in methods, although that means you can use methods and functions interchangeably.
_even so, there are cases where a.b() would not necessarily call b as a method of a;
in other words, there are cases when a.b() will not send a as b's "self" argument; like_:

```python
  class NoExplicit:
     def __init__(self):
        self.selfless = lambda: "nocomplain"

     def withself(): return "croak" #will throw if calld on an _instance_ of NoExplicit

  a = NoExplicit ()

  print(a.selfless()) #won't complain
  print(a.withself()) #will croak
```
_That is, even though an expression of the form a.b() looks like a method call, it is not always so ; arguably this is against the policy of "everything must be explicit and predictable as much as possible" python so vehemently tries adhering to ._
* Since tuples are represented with parentheses for clarity and disambiguation, it's a common misunderstanding that the parentheses are needed,
and part of the syntax of tuples. This leads to confusion, as tuples are conceptually similar to lists and sets, but the syntax is different.
It's easy to think it's the parentheses that makes it a tuple, when it is in fact the comma.
And if that wasn't already confusing enough, the empty tuple has no comma, only parentheses!
```python
  (1) == 1                      # (1) is just 1
  [1] != 1                      # [1] is a list
  1, == (1,)                    # 1, is a tuple
  (1) + (2) != (1,2)            # (1) + (2) is 1 + 2 = 3
  [1] + [2] == [1,2]            # [1] + [2] is a two-element list
  isinstance((), tuple) == True # () is the empty tuple
```
* [no labeled break or continue](http://www.python.org/dev/peps/pep-3136/)
* the body of lambda functions can only be an expression, no statements;
this means you can't do assignments inside a lambda, which makes them pretty useless
* no switch statement -- you have to use a bunch of ugly if/elif/elif ... tests, or ugly dispatch dictionaries (which have inferior power).
* the syntax for conditional-as-expression is awkward in Python (x if cond else y).  Compare to C-like languages: (cond ? x : y)
* no tail-call optimization, ruining the efficiency of some functional programming patterns
* no constants
* There are no interfaces, although abstract base classes are a step in this direction
* There are at least 5 different types of (incompatible) lists [https://jacobian.org/2007/mar/4/hate-python/](https://jacobian.org/2007/mar/4/hate-python/)
* Inconsistency between using methods/functions - some functionalities require methods (e.g. list.index()) and others require functions (e.g. len(list))
* No syntax for multi-line comments, idiomatic python abuses multi-line string syntax instead `"""`
* Function names with double underscore prefix and double underscore suffix are really really ugly `__main__`
* String character types in front of the string are really ugly `f.write(u'blah blah blah\n')`
* Python 3 allows you to annotate functions and their arguments, but they don't do anything by default.
Since there is no standard meaning for these, usage varies by toolkit or library, and you can't use it for two different things at once.
Python 3.5 tried to fix this by adding optional type hints to the standard library,
but since the majority of python is unannotated and you can only annotate functions, it's mostly useless.
* Generators are defined by using "yield" in a function body. If python sees a single yield in your function, it turns into a generator instead, and any statement that returns something becomes a syntax error.
* Python 3.5 introduces keywords async and await for defining coroutines.
Python pretended to have coroutines by abusing generators with yield and yield from, but instead of fixing generators, they added another thing.
Now there are asynchronous functions, as well as normal functions and generators,
and the rules for what keywords you can use inside of them are even more complex [https://www.python.org/dev/peps/pep-0492/#list-of-functions-and-methods](https://www.python.org/dev/peps/pep-0492/#list-of-functions-and-methods).
Unlike generators, where anything containing a yield becomes a generator, anything that uses coroutines has to be prefixed with async, including def, with and for.
* The super() call is a special case of the compiler that works differently if renamed [https://stackoverflow.com/a/19609168/539465](https://stackoverflow.com/a/19609168/539465)
* The standard library uses inconsistent naming schemes e.g.: `os.path.expanduser` and `os.path.supports_unicode_filenames`
(former does not separate words by underscore, while the latter does)


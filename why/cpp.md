---
title: C++
layout: language
date: 2017
author: theory.org
---
* It is backward compatible with C.
* But still with subtle differences that make some C code unable to compile in a C++ compiler.
* The standard libraries offer very poor functionalities compared to other languages' runtimes and frameworks.
* Too hard to implement and to learn: the specification has grown to over 1000 pages.
* Everybody thus uses a different subset of the language, making it harder to understand others' code.
* [Not suitable for low level system development](https://web.archive.org/web/20140111204548/http://msdn.microsoft.com/en-us/windows/hardware/gg487420.aspx#EFE) and quickly becomes a mess for user level applications.
* The standard does not define exception handling and name mangling. This makes cross-compiler object code incompatible.
* No widely used OS supports the C++ ABI for syscalls.
* What is 's', a function or a variable?
```cpp
  std::string s();
```
* * Answer: it's actually a function; for a variable, you have to omit the parentheses; but this is confusing since you use usually the parentheses to pass arguments to the constructor.
* * The value-initialized variable 's' would have to be:
```cpp
  std::string s; /* or */ std::string s{};
```
* There is **try** but no **finally**.
* * Considered a "feature" because RAII is supposed to be the main means of resource disposal in C++. Except a lot of libraries don't use RAII.
* Horrid Unicode support.
* Operators can be overloaded only if there's at least one class parameter.
* * This also makes impossible concatenating character array strings, sometimes leading programmers to use horrible C functions such as `strcat`.
* `catch (...)` doesn't let you know the type of exception.
* `throw` in function signatures is perfectly useless.
* `mutable` is hard to use reasonably and, since it fucks up `const` and thus thread safety, it can easily bring to subtle concurrency bugs.
* Closures have to be expressed explicitly in lambda expressions (never heard about anything like that in any functional language).
* * You can use `[=]` and enclose everything, but that still adds verbosity.
* The nature of C++ has led developers to write compiler dependent code, creating incompatibility between different compilers and even different versions of the same compiler.
* An `std::string::c_str()` call is required to convert an `std::string` to a `char*`. From the most powerful language ever we would have all appreciated a  overloaded `operator const char* () const`.
* Developers may have to worry about optimization matters such as whether declaring a function `inline` or not; and, once they've decided to do it, it is only a suggestion: the compiler may decide that was an incorrect suggestion and not follow it. What's the point? Shouldn't developers worry about optimization matters?
* * To rectify this nonsense many compilers implement `__forceinline` or similar.
* Templates are Turing-complete, hence compilers have to solve the halting problem (undecidable) to figure out whether a program will even compile.
* Unused global symbols do not generate any warnings or errors, they are compiled and simply increase the size of the generated object file.
* Changes that could actually standardise key functionality or substantially improve language usability [often](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4478.html)
[take](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4377.pdf) [a](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4514.pdf)
[TS](http://www.open-std.org/jtc1/s22/wg21/docs/papers/2016/n4592.pdf) [detour](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0267r2.pdf)
[to](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2013/n3820.html) [nowhere](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf).
Technical Specifications seem to be where good ideas go to die, often for political reasons, or some country didn't like it.
* The fact `export` [was never supported](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2003/n1426.pdf)
means you have to include the entirety of a template in your library header.
Traditional [C](/why/c) (and most other C++ code) separates implementation and prototypes.
* Every problem with the language is resolved by adding a new feature.

---
title: C
layout: language
date: 2017
author: theory.org
---

* Manual memory management can get tiring.
* String handling is manual memory management. See above.
* The support for concurrent programming is anemic.
* Terribly named standard functions: isalnum, fprintf, fscanf etc.
* The preprocessor.
* Not feeling your smartest today? Have a segfault.
* Lack of good information when segfault occurs... the "standard" GNU toolchain doesn't tell you the causes of segfaults in the standard runtime.
* If that code is legacy, you're in for two days of not looking like a hero.
* The standard library only covers basic string manipulation, file I/O, and memory allocation. And `qsort()` for some reason. You get nothing else that's truly portable.
* * Rolling your own containers (likely inferior to a standard, highly-optimised version) is something you better get used to doing for every new application, or using overly complicated existing frameworks that take over your application (nspr, apr, glib...).
* * There is also no standard for network programming. Most platforms use the Berkeley sockets API, but it varies wildly between vendors. Microsoft in particular has many striking differences in their API. Good luck finding a portable O(1) asynchronous socket selection API though; there's none (`select()` and `poll()` have O(n) behaviour).
* Variables are not automatically initalised to zero, unless they're static, because "it's more efficient."
* Want to use C99 portably? Sure, but if you need to use MSVC older than 2015 (very likely at some dev houses), you're screwed.
* Want to use C11 portably? Sure, but pretty much nothing besides GCC and clang support it. Sorry MSVC users.
* Variable length arrays are usually placed on the stack, meaning that passing in too large of a size is unsafe;
it will just stack overflow the moment you use it, made worse by the fact there is no way to tell your allocation is too big.
* malloc() takes one size parameter (size in bytes), but calloc() takes two (number of elements and element size in bytes).
* Undefined behaviour is the name of the game in C.
By undefined they mean "welcome to do whatever the vendor wants, and not necessarily what you want."
The worst part is [it's impossible to reliably find all undefined behaviour in a large program](http://blog.llvm.org/2011/05/what-every-c-programmer-should-know_14.html).
* * `int a = 0; f(a, ++a, ++a);` is not guaranteed to be `f(0, 1, 2)`. The order of evaluation is undefined.
* * Another surprise that bites people: `i[++a] = j[a];`, since which side is evaluated first is undefined.
* * Signed overflow is technically undefined behaviour. It just happens to work on most platforms.
* * Shifting by more than N bits on an intN_t type is undefined behaviour.
* * Casting an `int *` to a `float *` then dereferencing it is undefined behaviour. You have to use `memcpy()`.
* * Dereferencing a NULL pointer is undefined behaviour. It doesn't have to trap - you can actually get valid memory out of it!
* * Casting between function pointers and data pointers is technically undefined behaviour; on some platforms, they're different sizes (why a programmer needs to care in C is left as an exercise for the reader).
* * Casting `void (*)()` to `int (*)(int, const char *)` should be undefined right? Nope! All function pointers can be cast to one another. Actually calling the function after the cast if it is not the correct type is undefined behaviour, though (as you'd rightfully expect).
* * Casting something like, say, `FILE *` to `double *` and back again is _not_ undefined, so long as you don't dereference the pointer in-between. All data pointers are equivalent.
* Debugging optimised code can sometimes make no sense due to aggressive compiler optimisation.
* Data safety in multi-threaded programs is not really guaranteed by the language; things you think are atomic aren't necessarily without using (C11, but see above) atomics. Getting torn reads and writes is extremely easy, and no tools will ever warn you it's happening.
* void pointers are a drag; there's no way to determine what is actually behind them without some sort of user-defined tagging scheme that by definition cannot comprise all types. The solution is apparently "be careful."
* The type system in general is useless since you can cast (almost) anything to anything else.


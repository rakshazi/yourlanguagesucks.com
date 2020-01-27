---
title: Objective C
layout: language
date: 2017
author: theory.org
---
* No real use outside of programming for OS X and iOS that can't be done by another C-derived language,
meaning your skill set doesn't extend past a certain market.
* * Using Objective-C portably is an oxymoron; GNUStep is a window manager for the fringe on Linux, and there's nothing really there for Windows at all...
about the only thing that works is the compiler.
* No operator overloading.
* A "work-around" exists for method overloading.
* Tries to shoehorn the dynamically-typed Smalltalk into the statically-typed C.
* No stack-based objects.
* Syntax is very strange compared to other languages:
* * I have to put a `@` before the quotes when making a string?!
* * Methods are called like this unless you have a single argument?!? `[methodName args];`
* [http://fuckingblocksyntax.com](http://fuckingblocksyntax.com)
* There is no specification at all. No one (except maybe some LLVM developers and Apple) really knows what's going on under the hood.
* Could randomly crash if you return SEL from method [http://stackoverflow.com/a/20058585/1437441](http://stackoverflow.com/a/20058585/1437441)
* Awful type system. Types are more recommendations than types.
* Objective-C++ and the unholy horrors that arise from that
* * Objective-C and [C++](/why/cpp) classes can't inherit from one another.
* * [C++](/why/cpp) namespaces cannot interact with Objective-C code at all.
* * [C++](/why/cpp) pass by value can't be applied to Objective-C objects in C++ functions; you have to use pointers.
* * [C++](/why/cpp) lambdas and Objective-C blocks are different and not interchangeable.
* * Objective-C classes can't have members that are C++ classes that lack a default constructor, or that have one or more virtual methods...
except via pointers and allocating via new.
* Has no namespaces and instead encourages to use prefixes (two characters mostly) every class' name to prevent naming collision
* Objective-C has no standard library. Apple controls the de-facto one, and everything's prefixed with `NS*`.
* Apple's focus on Swift (which is itself yet another platform-specific language) means Objective-C is being left to rot on the vine.

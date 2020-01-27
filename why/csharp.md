---
title: C#
layout: language
date: 2017
author: theory.org
---

* ECMA and ISO standards of C# have been updated since C# 2.0, since then only Microsoft's specification exists.
* `i++.ToString` works, but `++i.ToString` does not.
* Parameters are not supported for most properties, only indexers, even though this is possible in Visual Basic .NET.
* The conventions are difficult to code review and deviate from other popular language conventions of similar style:
* * Almost everything is in pascal case (SomeClass, SomeConstantVariable, SomeProperty)
* * Unable to differentiate between 'extends' and 'implements' without using Hungarian-like notation such as IInterface
* Promotes slop (variant types and LINQ, while powerful adds a lot of clutter)
* "out" parameters, with syntax sugar.
* You can't override a virtual or abstract property that has a getter OR a setter with one that has a getter AND a setter,
making life quite difficult in the realm of property inheritance.
* * This does not apply to interfaces though, [read](http://stackoverflow.com/questions/82437/why-is-it-impossible-to-override-a-getter-only-property-and-add-a-setter).
* You can't perform any operations, even simple arithmetic, with objects inside a generic method (e.g. `T plus<T>(T t1, T t2) { return t1+t2; }`)
* You can't assign a new value inside a foreach loop (e.g. `foreach(int i in vec) { i = i+1; }`).
* [Implementing IDisposable properly](https://msdn.microsoft.com/en-us/library/fs2xkftw.aspx) is too complicated.
In particular, a naive implementation will not dispose if the finalizer is called by the garbage collector.
* Inconsistent numeric types: `short` is signed and `ushort` is unsigned; `int` is signed and `uint` is unsigned;
`long` is signed and `ulong` is unsigned — but `byte` is **unsigned** and `sbyte` is signed.

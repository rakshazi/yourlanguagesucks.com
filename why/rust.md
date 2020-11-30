---
title: Rust
layout: language
date: 2020
author: theory.org
---
# Safety

* Borrowing rules are more strict than what’s possible to safely do.
* The rules of unsafe are not strictly defined.
* LLVM's optimizer considers undefined behavior a license to kill.
Of course, that only matters in unsafe code, but you need unsafe code for anything complicated.

# API design and the type system #

* Overly terse named types and keywords that don't communicate their purpose, like `Vec` and `Cell`.
* Rust has two main string types and four other string types, for a total of six.
There are `String` and its slice equivalent `str` (“native” Rust UTF-8 string types used most of the time);
`CString` and `CStr` (when compatibility with [C](/why/c) is required); `OsString` and `OsStr` (when working with the OS’s String).
* Not smart enough coercion means that sometimes, you must use things like `&*some_var` (which will convert a smart pointer to a reference).
* You cannot use non-`Sized` types (like `str` and `[T]`) in several places, because every generic requires `Sized` unless you opt out by requiring `?Sized`.

# Toolchain

* rustc is slow.
* Because it statically links everything, you get outdated copies of several libraries on your computer.
* Actually, it statically links _almost_ everything.
It dynamically links your program to libc (unless you target [musl](https://www.musl-libc.org/), an alternative libc),
so your executables aren't really self-contained.
* Modifying a file in your project or updating a dependency requires you to recompile everything that depends on it.
* Type-ahead auto-completion is still a work in progress, because rustc is slow.
* IDE support is lacking.
* Generic types are very popular, and they're essentially copy-and-pasted for every concrete type they're used with.
Compiling essentially the same code over and over again is painful when rustc is slow.
* The optimizer [will break your program](https://github.com/rust-lang/rust/issues/28728) and it will run dog slow if you turn it off.
* Error messages from nested macros are difficult to understand.

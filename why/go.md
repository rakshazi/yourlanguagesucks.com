---
title: Go
layout: language
date: 2020
draft: true
author: theory.org
---
# Core language #

* Go supports the nil pointer. This is similar to [C](/why/c)'s `void *`, an enormous source of bugs.
Since nil can represent any type, it completely subverts the type system.

```go
func randomNumber() *int {
	return nil
}

func main() {
	a := 1
	b := randomNumber()
	fmt.Printf("%d\n", a+*b) // Runtime panic due to nil pointer
}
```

* Because strings are just slices of bytes, there is no simple way to index or slice a string if it contains non-ASCII characters.
You have to cast the string to a slice of "runes" (who calls characters that?), and then index that slice.
* Despite the above, Go has two competing types for representing text, `string` and `[]byte`.
`string`s are far superior to `[]byte`s, because they give Unicode characters when iterated over, can be compared with `== > < `,
can be conveniently concatenated with `+`, and can be used as map keys.
But important standard interfaces like `io.Writer` use `[]byte`.
* Similarly, the `len` function on strings returns the number of bytes in the string, which is not necessarily the number of characters.
To get a string's true length, you have to use the well-named but horribly verbose library function `utf8.RuneCountInString()`
* Even though Go does not require `break` at the end of every `case`, the `break` statement still breaks out of `switch` statements.
The same logic can always be accomplished by other control structures,
but you need to resort to a labelled `break` to exit a loop while inside a `switch`.
* Deleting the **n**th element from a slice sure doesn't look like deletion: `slice = append(slice[:n], slice[n+1:]...)`
* Indexing into a map with a key that doesn't exist silently returns the zero value for the type of the values.
To determine whether that was an actual value, you need to return a second boolean variable.
* If you import a library or declare a variable, but do not use it, your program will not compile even if everything else is valid.
* Composing functions that return multiple types is a mess.
* Go's error type is simply an interface to a function returning a string.
* Go lacks pattern matching & abstract data types.
* Go lacks immutable variables.
* Go lacks generics.
* Go lacks exceptions, and instead uses error codes everywhere, leading to repetitive error-checking boilerplate:

```go
if v, err := otherFunc(); err != nil {
	return nil, err
} else {
	return doSomethingWith(v), nil
}
```

* Actually, Go supports **one** kind of exception, but calls it a panic. You can catch an exception, but Go calls it recovering.
You can write "finally" blocks that run whether the function is exited by an exception or normally, but Go calls them deferred functions,
and they run in reverse order from how they're written.
* Go's `range` loop iterates over a collection, assigning its keys and/or values to some number of variables,
but the number of variables allowed and their meanings depend on the type of the collection, which might not be spelled out in the source code:

```go
d := loadData()

for i := range d {
	// if d is a channel, i is an element read from the channel
	// if d is a map, i is a map key
	// otherwise, i is an array/slice/string index
}

for i, j := range d {
	// if d is a channel, this is illegal!
	// (even though when receiving normally from a channel,
	//  you can get a second boolean value indicating whether it is closed)
	// if d is a string, i is an index and j is a rune (not necessarily d[i])
	// otherwise, i is an array/slice index or map key and j is d[i]
}
```
* Go lets you give names to return values so you can implicitly return them.
It also lets you redefine variables in inner scopes, shadowing the definitions in outer scopes.
That can make for unreadable code on its own, but the interaction between these two language features is just bizarre:

```go
func foo1(i bool) (r int) {
	if i {
		r := 12
		fmt.Println(r)
	}
	return  // returns 0
}

func foo2(i bool) (r int) {
	if i {
		r = 12
		fmt.Println(r)
		return  // returns 12
	}
	return  // returns 0
}

func foo3(i bool) (r int) {
	if i {
		r := 12
		fmt.Println(r)
		return  // ERROR: r is shadowed during return
	}
	return
}
```

* Deferred functions can write to a function's named return values, leading to surprises:

```go
func dogma() (i int) {
	defer func() {
		i++
	}()
	return 2 + 2
}

func main() {
	fmt.Println(dogma())  // prints 5
}
```

* Comparing an interface with nil checks whether the **type** of the interface is nil, and not its value. Thus forms a trap that has snared every Go programmer:

```go
func returnsError() error {
	var p *MyError = nil
	if bad() {
		p = ErrBad
	}
	return p // Will always return a non-nil error.
}
```

# Concurrency

* If multiple channels are available to receive from or send to, the `select` statement picks a `case` at random,
meaning that to prioritize one channel over another you have to write:

```go
select {
case <-chan1:
	doSomething()
default:
	select {
	case <-chan1:
		doSomething()
	case <-chan2:
		doSomethingElse()
	}
}
```
* The `select` statement is implemented as [about 600 lines of runtime code](https://github.com/golang/go/blob/master/src/runtime/select.go).
You can almost feel the performance decrease every time you use one.
* Goroutine leaks, where a goroutine loses all its ways of communicating with others, can leak whole **stacks** of memory that can't be garbage-collected.

# Standard library

* Date/time format strings don't use the type of mnemonic codes seen in other languages, like "ddd" for abbreviated day or "%H" for hour.
Instead, Go's library uses a system where "1" means month, "5" means seconds, "15" means hour, "6" means year, and so on.
The documentation explains it in terms of a magic reference time (Mon Jan 2 15:04:05 MST 2006) and says
"To define your own format, write down what the reference time would look like formatted your way".
But it doesn't actually figure out how to transform times to meet your specifications;
it simply recognizes a fixed, undocumented set of codes.
* Two implementations of random numbers, `math/rand` and `crypto/rand`.
* The `flag` package, which implements command-line flags, is not POSIX-compliant and doesn't support shorthand flags.
* The `errors` package is twenty lines long, because Go's error type is simply an interface to a function returning a string.

# Toolchain

* Go's native package system does not support specifying versions or commits in the dependency information.
Instead, the go community recommends that each major release have its own separate repository; github.com/user/package/package-{v1,v2,v3}.
* Go's native package system does not support dependencies, forcing the community to create [a lot of alternatives](https://github.com/golang/go/wiki/PackageManagementTools).
* Version 1.5 converted the entire toolchain from C to Go.
Because that was mostly done automatically, it caused significant performance decreases at both compile time and runtime.
* The compiler has an option documented as "reject unsafe code".
All it does is prevent you from importing the `unsafe` package or any package that imports it recursively,
which includes virtually the entire standard library, making any program you can compile with that option incapable of output and thus useless.
* The linker also has such an option, only it prevents compiling **any** program, because the Go runtime depends on `unsafe`.

# Community

* Most complaints, especially about error handling, are brushed off as "you don't understand the language well enough".
* The contributors don't listen to the community.
* * Take [this proposal to add aliases](https://github.com/golang/go/issues/16339),
which was pushed through despite [lots of negative feedback](https://groups.google.com/forum/#!topic/golang-dev/OmjsXkyOQpQ).
And when it was rolled back, it [wasn't because the community asked for it](https://github.com/golang/go/issues/16339#issuecomment-258531805),
but because there were problems even the contributors hadn't anticipated.
* * Or take [this feature request](https://github.com/golang/lint/issues/65), which was immediately closed as WONTFIX despite significant community demand
(though it was later implemented).
* Almost nothing in this list can be fixed, because of the [Go 1 compatibility promise](https://golang.org/doc/go1compat).
Until Go 2.0, that is, but [that may never happen](https://docs.google.com/presentation/d/1JsCKdK_AvDdn8EkummMNvpo7ntqteWQfynq9hFTCkhQ/view?slide=id.p#slide=id.p).


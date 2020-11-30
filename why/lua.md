---
title: Lua
layout: language
date: 2017
authors: theory.org and big-lip-bob
---

# Syntax

* Variable declaration is global by default, and looks exactly like assignment.

```lua
do
  local realVar = "foo"
  real_var = "bar" -- Oops
end
print(realVar, real_var) -- nil, "bar"
```

* A dereference on a non-existing key returns `nil` instead of an error. This, coupled with the above point makes misspellings hazardous, and mostly silent, in Lua.
It is possible to overload deferencing of non-existing keys and the declaration of new global variables by adding metamethods,
and the Global variables are part of the Global table `_G` which can have its operators overloaded too.

```lua
setmetatable(_G,{
  __newindex = function(self,key,value) error("tried to add a new global : "..key.." = "..value) end,
  __index = function(self,key) error("Tried to acces non existing global : "..key) end
})
a = 5 -- error, new global added
print(b) -- error, empty global read
```

* `break`, `do while` (`while something do` and  `repeat something until something`), and `goto` exist, but not `continue`. Bizzare.
* Statements are distinct from expressions, and expressions cannot exist outside of statements:

```lua
do 2+2 end -- unexpected symbol near '2'
do return 2+2 end -- 4
```

# Types

* If a vararg is in the middle of a list of arguments only the first argument gets counted.

```lua
local function fn() return "bar", "baz" end
print("foo", fn()) -- foo bar baz
print("foo", fn(), "qux") -- foo bar qux
```

* You can hold only one vararg at a time (in `...`) 
* You can't mutate varargs directly.
* Logical operators `and` | `or` cut off varargs, this is especially annoying when working with recursive functions (Lua has TCO).
* You can't easely store varargs for later. You can create closures or pack varargs into tables to do these things,
but then you have to worry about escaping the nil values, which are valid in varargs but signal the end of tables, like `\0` in C strings.

```lua
local as_closure = function(...) return ... end -- function variant -- expensive ?
local as_table   = {n = select('#',...),...} -- table variant -- n field to indicate the last vararg index
```

* Table indexes start at one in array literals, and in the standard library.
You can use 0-based indexing, but then you can't use either of those things.

* Lua's default string library provides only a subset of regular expressions, that is itself incompatible with the usual PCRE regexes.
* No default way to copy a table. You can write a function for that which will work till you'll want to copy a table with `__index` metamethod.
* No way to impose constraints on function arguments. 'Safe' Lua functions are a mess of type-checking code.
* Lack of object model. Not bad by itself, but it leads to some inconsistencies - the string type can be treated like object,
assigned a metatable and string values called with methods. The same is not true for any other type.

```lua
>("string"):upper()
  STRING
>({1,2,3}):concat()
  stdin:1: attempt to call method 'concat' (a nil value)
>(3.14):floor()
  stdin:1: attempt to index a number value
```

# Toolchain

* Lua doesn't have an offical toolchain as its an embedable language
* LuaRocks exists but is too hard to setup, especially on Windows

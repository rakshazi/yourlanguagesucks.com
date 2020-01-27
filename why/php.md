---
title: PHP
layout: language
date: 2017
author: theory.org
---

* `'0'`, `0`, and `0.0` are false, but `'0.0'` is true
* Because it's [a fractal of a bad design](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/) and it [makes me sad](http://phpsadness.com/).
* The documentation is not versioned. There is a single version of the docs that you are supposed to use for php4.x, php5, php5.1...
* There is no general concept of an identifier. Some identifiers (like variable names) are case sensitive, others case insensitive (like function calls):

```php
 $x = Array();
 $y = array();
 $x == $y; # is true
 $x = 1;
 $X = 1;
 $x == $X; # is true
```

* if you mis-type the name of a built-in constant, a warning is generated and it is interpreted as a string "nonexistent_constant_name". Script execution is not stopped.
* if you send too many arguments to a user-defined function call, no error is raised; the extra arguments are ignored.
** This is intentional behavior for functions that can accept variables numbers of arguments (see `func_get_args()`).
* if you send the wrong number of arguments to a built-in function call, an error is raised; the extra arguments aren't ignored such as with a normal function call
* `Array()` is a hash & array in one data type
* "hash & array" would be relatively ok. ''ordered'' hash & array, now, that's pure evil. Consider:

```php
 $arr[2] = 2;
 $arr[1] = 1;
 $arr[0] = 0;
 foreach ($arr as $elem) { echo "$elem "; } // prints "2 1 0" !!
```

* it doesn't have dynamic scoping
* in addition to implementing POSIX STRFTIME(3), roll-your-own [date](http://php.net/manual/en/function.date.php) formatting language.
* weakass string interpolation resolver:

```php
  error_log("Frobulating $net->ipv4->ip");
  Frobulating  Object id #5->ip

  $foo = $net->ipv4;
  error_log("Frobulating $foo->ip");
  Frobulating 192.168.1.1
```

However, this will work as expected:

```php
  error_log("Frobulating {$net->ipv4->ip}");
```

* there are two ways to begin end-of-line comments: `//` and `#`
* two names for the same floating point type: `float` and `double`
* an integer operation overflow automatically converts the type to float
* There are thousands of functions. If you have to deal with arrays, strings, databases, etc.,
you'll get with tens of functions such as `array_diff`, `array_reverse`, etc.
Operators are inconsistent; you can only merge arrays with +, for example (- doesn't work).
Methods? No way: `$a.diff($b), $a.reverse()` don't exist.
_PHP is a language based on C and Perl, which is not inherently object oriented. And if you know the internals for objects, this actually makes you happy._
* function names aren't homogeneous: `array_reverse` and `shuffle` both operates on arrays
* and some functions are needle, haystack while others are haystack, needle.
* string characters can be accessed through both brackets and curly brackets
* variable variables introduce ambiguities with arrays: `$$a[1]` must be resolved as `${$a[1]}` or `${$a}[1]` if you want to use `$a[1]` or `$aa` as the variable to reference to.
_Variable Variables in general are a great evil. If variable variables are the answer, you are surely asking the wrong question._
* constants do not use `$` prefix, like variables - which makes sense, since they're constants and not variables.
* `!` has a greater precedence over `=` but... not in this `if (!$a = foo())` "special" case!
* on 32 and 64 bit systems shift operators (`<< >> <<= >>=`) have different results for more than 32 shifts
* (instances of) built-in classes can be compared, but not user-defined ones
* arrays can be "uncomparable"
* `and` and `or` operators do the same work of `&&` and `||`, but just with different precedence
* there are both `(int)` and `(integer)` to cast to integers, `(bool)` and `(boolean)` to cast to booleans, and `(float)`, `(double)`, and `(real)` to cast to floats
* casting from float to integer can lead to undefined results if the float is beyond the boundaries of integer
* included file inherits the variable scope of the line on which the include occurs, but functions and classes defined in the included file have global scope
_class and function definitions are always global scope if namespace not explicity set_
* functions and classes defined within other functions or classes have global scope: they can be called outside the original scope _if namespace not set_
* defaults in functions should be on the right side of any non-default arguments, but you can define them everywhere leading to unexpected behaviours:
```php
  function makeyogurt($type = "acidophilus", $flavour)
  {
    return "Making a bowl of $type $flavour.\n";
  }

  echo makeyogurt("raspberry"); // prints "Making a bowl of raspberry ". Only a warning will be generated
```
* if the class of an object which must be unserializing isn't defined when calling `unserialize()`,
an instance of `__PHP_Incomplete_Class` will be created instead, losing any reference to the original class
* `foreach` applied to an object iterates through its variables by default
* nested classes (a class defined within a class) are not supported
```php
  class a {
     function nextFoo() {
        class b {} //not supported
     }
  }
```
* globals aren't always "globals":
```php
  $var1 = "Example variable";
  $var2 = "";
  function global_references($use_globals)
  {
    global $var1, $var2;
    if (!$use_globals) {
      $var2 =& $var1; // visible only inside the function
    } else {
      $GLOBALS["var2"] =& $var1; // visible also in global context
    }
  }
  global_references(false);
  echo "var2 is set to '$var2'\n"; // var2 is set to ''
  global_references(true);
  echo "var2 is set to '$var2'\n"; // var2 is set to 'Example variable'
```
* there's no module / package concept: only include files, "ala C" _Much PHP functionality is provided via compiled modules written in C._
* no way to store 64-bit integers in a native data type on a 32-bit machine,
leading to inconsistencies (intval('9999999999') returns 9999999999 on a 64-bit machine, but returns 2147483647 on a 32-bit machine)
* The class that represents a file is called: `SplFileObject`.
* `SplFileObject` both extends and is a property of the file meta object `SplFileInfo`. Can't choose between inhertitance and composition? LETS USE **BOTH**!?
* this makes my head explode:
```php
in_array("foobar", array(0)) == true
```
_This is because when doing non-strict comparisons, the string "foobar" is coerced into an integer to be compared with the 0. You need to use the strict flag for `in_array` to work as expected, which uses === instead of ==, apparently. You know, for true equality, not just kind of equality._
* php.ini can change the whole behavior of your scripts thus not making them portable between different machines with different settings file.
* `null`, `''`, `0`, and `0.0` are all equal.



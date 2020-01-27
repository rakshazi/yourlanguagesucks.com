---
title: Perl 5
layout: language
date: 2017
author: theory.org
---
* _Perl is worse than Python because people wanted it worse_. Larry Wall, 14 Oct 1998
* use strict -- really, it should be 'use unstrict' or 'use slop' (as in billiards/pool) that changes things, and that itself should never ever be used.
Ever. By anyone. 'strict' mode should be the default.

```perl
  use strict;
  use warnings;
```

* Sigil variance is hugely annoying

```perl
  my @list = ("a", "b", "c");
  print $list[0];
```

* no parameter lists (unless you use Perl6::Subs)

```perl
  sub foo {
    my ($a, $b) = @_;
  }
```

* dot-notation for methods, properties, etc is a good thing, especially when so many other C-styled languages do it and Perl is one of the random ones that doesn't.
* The type system should be revamped. There is only support for (Scalar, Array, Hash, etc) and not support for (Int, Str, Bool, etc).
* It is too hard to determine the type of a Scalar, e.g there is no easy way to determine if a variable is a string.
* Just try explaining a ref to a C programmer. They will say "oh, it's a pointer!" except it isn't. Not really.
It's _like_ a pointer, or so I hear. As a Perl programmer, I know that it's not quite a pointer,
but I don't know exactly what a pointer is (while I do know what a ref is, but I can't explain it -- neither could Larry Wall: He called it a 'thingy').
* The regular expression syntax is horrid.
* there's rarely a good place to put the semicolon after a here-doc.

```perl
 my $doc = <<"HERE";
    But why would you?
 HERE
 print $doc;
```

* it generally takes you ten years to figure out how to call functions or methods inside double-quoted strings like any other variable.
If you're reading this, though, you get a break: `You do it @{[sub{'like'}]} this. Easy`
* just like Ruby it has redundant keyword "unless". You can do "if not" at least in three ways, which can lead to confusion and spoiling readability of code:
* * if(!expression)
* * if(not expression)
* * unless(expression)
* i cant say `if($a==$b) $c=$d ;` instead i have to say either:
* * $c=$d if($a==$b) ;  or
* * if($a==$b) { $c=$d; }


* what's with all the $,@,%,& things in front of variables?
It takes a lot of effort to type those redundancies every single time ...
C and most others let you define a type just once, and then you can use it without reminding yourself what it is.
In any case, that Perl lets you change the thing depending on what you want to do so it's even more stupid to use say @ and $ in front of the same var.
* You will not understand your program when you revisit it 6 months later.

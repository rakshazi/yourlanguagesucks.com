---
layout: page
title: Add new language that
---
{% include beta.html %}

[{{ site.title }}](/) &rarr; [add new page](/new#how-to-add-new-language-page) or [update existing page](/new#how-to-update-existing-page)

# How to add new language page

* Open [new file page on github]({{ site.repo}}new/master/why)
* Set filename in format `<language>.md`, eg: `javascript.md`
* In file content set metadata (on top of the file) structure:

```yml
---
layout: language
title: <Language Name>
author: <your name>
---
```

Example:
```yml
---
title: PHP
layout: language
date: 2017
author: theory.org
---
```

* Fill reasons why `<language>` sucks in list format:

```markdown
* Reason 1
* Reason 2
* Reason 3

echo("code listings with examples are good idea!\n);

...
```

Example ([result on website](/why/php)):

```markdown
* `'0'`, `0`, and `0.0` are false, but `'0.0'` is true
* Because it's [a fractal of a bad design](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/) and it [makes me sad](http://phpsadness.com/).
* The documentation is not versioned. There is a single version of the docs that you are supposed to use for php4.x, php5, php5.1...
* There is no general concept of an identifier. Some identifiers (like variable names) are case sensitive, others case insensitive (like function calls):

 $x = Array();
 $y = array();
 $x == $y; # is true
 $x = 1;
 $X = 1;
 $x == $X; # is true
```

aaaand create a Pull Request. Your submission will be reviewed and merged to website ASAP.

# How to update existing page

* Open page that you want to update (add new "fucks", remove outdated "fucks" or rearrange them), example: [JavaScript](/why/javascript)
* Click on **Edit on GitHub** link on top right or bottom right corners
* Make changes...
* aaaand  create a Pull Request. Your submission will be reviewed and merged to website ASAP.

# trim

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Trims leading and trailing characters.{.purpose}

~~~
R=[X] trim Y
~~~

X is a scalar Char, and defaults to blank. Y is Char. R is Y with the leading and trailing
characters defined in X removed.

If X is single character, then all leading an trailing characters that equal X are removed. If X is
a string of N (2 or more) characters, then the first and last N characters are removed if they
match X.

See also: →[trimLeading], →[trimTrailing], →[take], →[drop]

## Examples

~~~
      a='   paul    '
      a
┌────────┐
│   paul │
└Char(11)┘
      trim a
┌───────┐
│paul   │
└Char(4)┘
      b='  Paul,Phil      ,Steven'
      b
┌────────┐
↓  Paul  │
│Phil    │
│Steven  │
└Char(10)┘
      trim b
┌───────┐
↓Paul   │
│Phil   │
│Steven │
└Char(6)┘
~~~


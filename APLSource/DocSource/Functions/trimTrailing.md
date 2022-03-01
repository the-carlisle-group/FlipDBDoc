# trimTrailing

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Trims trailing characters.{.purpose}

~~~
R=[X] trimTrailing Y
~~~

X is a scalar Char, and defaults to blank. Y is Char. R is Y with the trailing characters defined
in X removed.

If X is single character, then all trailing characters that equal X are removed. If X is a string
of N (2 or more) characters, then the last N characters are removed if they match X.

See also: →[trim], →[trimLeading], →[take], →[drop]

## Examples

~~~
      trimTrailing '  Hello  '
┌───────┐
│  Hello│
└Char(7)┘
      '0' trimTrailing '123000'
┌───────┐
│123    │
└Char(3)┘
      '00' trimTrailing '123000'
┌───────┐
│1230   │
└Char(4)┘
      '000' trimTrailing '123000'
┌───────┐
│123    │
└Char(3)┘
      '0000' trimTrailing '123000'
┌───────┐
│123000 │
└Char(6)┘
~~~


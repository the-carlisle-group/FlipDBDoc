# trimLeading

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Trims leading characters.{.purpose}

~~~
R=[X] trimLeading Y
~~~

X is a scalar Char, and defaults to blank. Y is Char. R is Y with the leading characters defined in
X removed.

If X is single character, then all leading characters that equal X are removed. If X is a string of
N (2 or more) characters, then the first N characters are removed if they match X.

See also: →[trim], →[trimTrailing], →[take], →[drop]

## Examples

~~~
      trimLeading '    Hello'
┌───────┐
│Hello  │
└Char(5)┘
      '0' trimLeading '0001234'
┌───────┐
│1234   │
└Char(4)┘
      '00' trimLeading '0001234'
┌───────┐
│01234  │
└Char(5)┘
      '000' trimLeading '0001234'
┌───────┐
│1234   │
└Char(4)┘
      '0000' trimLeading '0001234'
┌───────┐
│0001234│
└Char(7)┘
~~~


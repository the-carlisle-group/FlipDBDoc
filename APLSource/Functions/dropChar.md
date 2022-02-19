# dropChar

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Truncate strings.{.purpose}

~~~
R=X dropChar Y
~~~

Y is Char. X is a scalar integer.

R is Char and is equal to Y with the first X characters removed if X is positive, or the last X
characters removed if X is negative.

See also: →[takeChar]

## Examples

~~~
      a='00012399,00045699,11134577'
      a
┌────────┐
↓00012399│
│00045699│
│11134577│
└Char(8)─┘
      3 dropChar a
┌───────┐
↓12399  │
│45699  │
│34577  │
└Char(5)┘
      -2 dropChar a
┌───────┐
↓000123 │
│000456 │
│111345 │
└Char(6)┘
~~~


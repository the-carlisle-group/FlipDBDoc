# toUpper

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Enforces uppercase.{.purpose}

~~~
R=toUpper X
~~~

X is Char. R is X with all lowercase characters converted to uppercase.

See also: →[toLower]

## Examples

~~~
      toUpper 'abc'
┌───────┐
│ABC    │
└Char(3)┘
      toUpper 'FlipDB,E.F. Codd'
┌─────────┐
↓FLIPDB   │
│E.F. CODD│
└Char(9)──┘
~~~


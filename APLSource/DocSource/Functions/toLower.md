# toLower

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Enforces lowercase.{.purpose}

~~~
R=toLower X
~~~

X is Char. R is X with all uppercase characters converted to lowercase.

See also: →[toUpper]

## Examples

~~~
      toLower 'ABC'
┌───────┐
│abc    │
└Char(3)┘
      toLower 'FlipDB,E.F. Codd'
┌─────────┐
↓flipdb   │
│e.f. codd│
└Char(9)──┘
~~~


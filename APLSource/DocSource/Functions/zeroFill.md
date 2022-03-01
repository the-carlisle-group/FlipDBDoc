# zeroFill

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Replace blanks by zeros{.purpose}

~~~
R=zeroFill Y
~~~

Y must be Char. R is Char. All blanks are replaced by zeros. If there are no blanks R is Y.

## Examples

~~~
      zeroFill '   123'
┌───────┐
│000123 │
└Char(6)┘
     zeroFill -7 takeChar '123,456,789'
┌───────┐
↓0000123│
│0000456│
│0000789│
└Char(7)┘
~~~


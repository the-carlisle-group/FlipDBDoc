# catenate

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Join strings.{.purpose}

~~~
R=X catenate Y
~~~

X and Y must be Char columns. R is X concatenated with Y.

## Examples

~~~
      'abc' catenate 'def'
┌───────┐
│abcdef │
└Char(6)┘
      'Adam' catenate ' ' catenate 'Smith'
┌──────────┐
│Adam Smith│
└Char(10)──┘
      'One,Two,Three,Four' catenate '999'
┌────────┐
↓One999  │
│Two999  │
│Three999│
│Four999 │
└Char(8)─┘
~~~


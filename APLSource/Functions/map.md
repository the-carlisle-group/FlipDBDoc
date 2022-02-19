# map

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Misc{.info}

Alter values{.purpose}

~~~
R=map X Y Z
~~~

X is any column. Y and Z are corresponding "from" and "to" lists. Types
of Y and Z must be compatible with X. R is X where values that match an element of Y are replaced
with the corresponding element of Z. The type of R is determined as that of X or Z such that no
precision is lost.

## Examples

~~~
      a='NY,NJ,CA,NY'
      a
┌───────┐
↓NY     │
│NJ     │
│CA     │
│NY     │
└Char(2)┘
      map a 'NY,CA' 'New York,California'
┌──────────┐
↓New York  │
│NJ        │
│California│
│New York  │
└Char(10)──┘
~~~


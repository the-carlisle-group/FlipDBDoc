# coalesce

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Selection{.info}

Returns first non-zero or non-blank item in a column.{.purpose}

~~~
R=coalesce X
~~~

X may be any type except Blob. If X is numeric, R is the first non-zero item found in X. If X is
char, R is the first non-blank item found in X. If X is temporal, R is the first non-null date or
datetime found in X.

It is common to use the →[*.Operators.across] operator with coalesce to the apply the function
across multiple columns, rather than down a single column, simulating the behavior of coalesce in
Oracle and SQLServer.

## Examples

~~~
      coalesce  0 0 5 0 7 8 0
┌────┐
│5   │
└Int8┘
      c=',,Paul,Phil,Sam'
      c
┌───────┐
↓       │
│       │
│Paul   │
│Phil   │
│Sam    │
└Char(4)┘
      coalesce c
┌───────┐
│Paul   │
└Char(4)┘
      a=0 0 0 1 2
      b=0 4 0 5 6
      c=7 8 9 10 11
      a b c (coalesce across a b c)
 ┌────┐  ┌────┐  ┌────┐  ┌────┐
 ↓0   │  ↓0   │  ↓7   │  ↓7   │
 │0   │  │4   │  │8   │  │4   │
 │0   │  │0   │  │9   │  │9   │
 │1   │  │5   │  │10  │  │1   │
 │2   │  │6   │  │11  │  │2   │
 └Int8┘  └Int8┘  └Int8┘  └Int8┘
~~~


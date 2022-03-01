# mode

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical,Misc{.info}

~~~
R=mode X
~~~

X may be any column including Blob.

R is the (first) most frequently-occurring value.

## Examples

~~~
      N = 0 0 1 1 2 2 2 3 3 3 3 4 4 4 4 4
      (median N) (average N) (mode N)
 ┌────┐  ┌─────┐  ┌────┐
 │3   │  │2.5  │  │4   │
 └Int8┘  └Float┘  └Int8┘
      mode 'one,two,three,two,three'
┌───────┐
│two    │
└Char(5)┘
      mode emptyInteger
┌────┐
│0   │
└Int8┘
~~~


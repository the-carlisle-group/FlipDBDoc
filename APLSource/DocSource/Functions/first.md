# first

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Selection{.info}

First element.{.purpose}

~~~
R=first X
~~~

R is the first element of X.  R is the same type as X. X may be any column.

See also →[last], →[nth], and →[pick].

## Examples

~~~
      first 1 2 3
┌────┐
│1   │
└Int8┘
      first 'One,Two,Three'
┌───────┐
│One    │
└Char(5)┘
      first emptyInteger
┌────┐
│0   │
└Int8┘
~~~


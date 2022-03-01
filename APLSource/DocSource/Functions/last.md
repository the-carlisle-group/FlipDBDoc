# last

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Selection{.info}

Last element.{.purpose}

~~~
R=last X
~~~

R is the last element of X.  R is the same type as X. X may be any column including Blob.

See also →[first], →[nth] and →[pick].

## Examples

~~~
      last 1 2 3 4
┌────┐
│4   │
└Int8┘
      last 'Mike,Tim,Jon,Henry'
┌───────┐
│Henry  │
└Char(5)┘
      last emptyInt
┌────┐
│0   │
└Int8┘
      last emptyChar
┌───────┐
│       │
└Char(0)┘
~~~


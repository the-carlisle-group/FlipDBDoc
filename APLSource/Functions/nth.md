# nth

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Selection{.info}

nth element.{.purpose}

~~~
R=Y nth X
~~~

R is the Yth element of X.  R is the same type as X. X may be any column including Blob.
If there is no Yth element, R defaults to zero or blank.

See also →[first] and →[last].

See also →[pick], which is a structural function and operates on arrays.

## Examples

~~~
      3 nth 4 5 6 7 8 9
┌────┐
│7   │
└Int8┘
      0 nth 'One,Two,Three'
┌───────┐
│One    │
└Char(3)┘

      10 nth 'One,Two,Three'
┌───────┐
│       │
└Char(0)┘

      2 nth emptyInteger
┌────┐
│0   │
└Int8┘
~~~


# percentAcross

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Arithmetic{.info}

Computes percentage of subgroup within a group.{.purpose}

~~~
R=[Y] percentAcross X [B]
~~~

X may be numeric or Char.  B is a Boolean restriction. R is the percentage of X where B is true. Y
is integer and is an optional rounding factor. If X is character, the percentage is based on row count.

This function has special handling when used in the context of a cross-tab query.

While B is optional, it is only useful to omit when percentAcross is used under a cross-tab query.

See also: →[percentDown], →[percentOverall]

For numeric X this function is equivalent to:

~~~
Y round 100 * (sum X where B) / sum X
~~~

For Char X this function is equivalent to:

~~~
Y round 100 * (count X where B) / count X
~~~

## Examples

~~~
      D=10 20 10 5 5 30 20
      B=1  0  0  1 1 0  1
      sum D
┌────┐
│100 │
└Int8┘
      percentAcross D B
┌─────┐
│40   │
└Float┘
      percentAcross D (20 gt D)
┌─────┐
│30   │
└Float┘
      D='Tim,Mike,Tom,Terry,Jon,Len'
      percentAcross D (D like 'T%')
┌─────┐
│50   │
└Float┘
~~~


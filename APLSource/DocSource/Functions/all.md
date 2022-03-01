# all

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Logical{.info}

Logical all. Aggregate function.{.purpose}

~~~
R=all X
~~~

X must be boolean. R is Boolean. R is 1 if all the items of X are 1, else R is 0.

See also: →[and]

## Examples

~~~
      all 1 1 0 0
┌───────┐
│0      │
└Boolean┘
      all 1 1 1 1
┌───────┐
│1      │
└Boolean┘
      all 0 0 0 0
┌───────┐
│0      │
└Boolean┘
~~~


# any

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Logical{.info}

Logical any.{.purpose}

~~~
R=any X
~~~

X must be boolean. R is Boolean. R is 1 if any element of  X is 1, else R is 0.

See also: →[or]

## Examples

~~~
      any 0 0 0
┌───────┐
│0      │
└Boolean┘
      any 0 1 0 0 1
┌───────┐
│1      │
└Boolean┘
      any 1 1 1
┌───────┐
│1      │
└Boolean┘
      any 0 0 0 0 1 0
┌───────┐
│1      │
└Boolean┘
~~~


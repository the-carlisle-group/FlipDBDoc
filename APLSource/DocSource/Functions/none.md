# none

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Logical{.info}

Logical any.{.purpose}

~~~
R=none X
~~~

X must be boolean. R is Boolean. R is 0 if any element of X is 1, else R is 1.

See also: →[not] →[any]

## Examples

~~~
      none 0 0 0
┌───────┐
│1      │
└Boolean┘
      none 0 1 0 0 1
┌───────┐
│0      │
└Boolean┘
      none 1 1 1
┌───────┐
│0      │
└Boolean┘
      not any 0 0 0 0 1 0
┌───────┐
│0      │
└Boolean┘
~~~


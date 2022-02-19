# toBoolean

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=toBoolean X
~~~

Converts column X to type boolean. If X is of type char, the result of functions →[toNumeric]
actually defines the outcome.  X may be any type except Blob. R is 0 if X is 0, or converts to 0,
else R is 1.

Note that any lines which contain digits are likely to produce a result, because all non-digits but these:

~~~
.-¯
~~~

are removed.

## Examples

~~~
       toBoolean '0,1'
┌───────┐
↓0      │
│1      │
└Boolean┘
       toBoolean '0,1,5,Hello'
┌───────┐
↓0      │
│1      │
│1      │
│0      │
└Boolean┘
      toBoolean 0 1 2
┌───────┐
↓0      │
│1      │
│1      │
└Boolean┘
~~~


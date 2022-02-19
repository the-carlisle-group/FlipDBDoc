# proportion

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

Calculates the sample proportion{.purpose}

~~~
R=proportion X
~~~

X must be boolean. R is Float.  0<=R<=1.

## Examples

~~~
      proportion 1 1 0 1 1 1 0 1 1
┌────────────┐
│0.7777777778│
└Float───────┘
      proportion roll 100 replicate 2
┌─────┐
│0.52 │
└Float┘
~~~


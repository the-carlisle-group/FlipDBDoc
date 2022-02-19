# structure

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=structure X
~~~

X may be any column. R is an Integer scalar representing the structure of X.

If X is scalar, R is 0. If X is simple, R is 1. If X is enclosed, R is 2. If X is partitioned, R is 3.

See also: →[type]

## Examples

~~~
      S=1
      V=1 2 3
      E=enclose 1 2
      P=enclose (1 2)(3 4)
      structure each S V E P
 ┌────┐  ┌────┐  ┌────┐  ┌────┐
 │0   │  │1   │  │2   │  │3   │
 └Int8┘  └Int8┘  └Int8┘  └Int8┘
~~~


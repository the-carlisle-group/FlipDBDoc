# if

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Selection{.info}

~~~
R=if X Y Z
~~~

X, Y & Z are structurally conforming columns. Y & Z must be compatible types. X is Boolean.

R is formed from the items of Y and Z according to X; from Y where X is 1; from Z where X is 0.

The application of the function may be read as "if X then Y, else Z".

The type of R is determined as that of Y or Z such that no precision is lost.

## Examples

~~~
      a='CA,NY,CA,CT'
      a
┌───────┐
↓CA     │
│NY     │
│CA     │
│CT     │
└Char(2)┘
      if (a in 'CA') 'California' a
┌──────────┐
↓California│
│NY        │
│California│
│CT        │
└Char(10)──┘
~~~


# expand

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Expansion, insertion.{.purpose}

~~~
R=X expand Y
~~~

X is Integer. Y is any column. Note that expand does not work on object arrays. X has a positive
integer for every item in Y interspersed with additional zeros or negative integers.

R is type as of Y with new prototypical items inserted corresponding to the position and value of
negative items in X. Zeros in X are treated as (-1).

New items in R are prototypes, that is, empty strings or zeros according to type.

## Examples

~~~
      1 0 1 3 expand 1 2 3
┌────┐
↓1   │
│0   │
│2   │
│3   │
│3   │
│3   │
└Int8┘
       4 -2 2 expand 'Tim,Mike'
┌───────┐
↓Tim    │
│Tim    │
│Tim    │
│Tim    │
│       │
│       │
│Mike   │
│Mike   │
└Char(4)┘
~~~


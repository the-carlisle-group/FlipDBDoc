# integers

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=integers X
~~~

X is a scalar or simple list of non-negative integers. For scalar X, R is X consecutive integers
starting at 0 (zero). For a list X, R is partitioned.

## Examples:

~~~
      integers 4
┌────┐
↓0   │
│1   │
│2   │
│3   │
└Int8┘
      integers 0
┌────┐
↓Int8┘
      integers 7 3 5
┌───────────────┐
↓[0,1,2,3,4,5,6]│
│[0,1,2]        │
│[0,1,2,3,4]    │
└Int8───────────┘
~~~


# enlist

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=enlist X
~~~

X may be any column, or an array of columns of compatible type. R is a simple column containing all
the elements of X.

See also: →[enclose], →[disclose]

## Examples:

~~~
      enlist 5
┌────┐
↓5   │
└Int8┘
      enlist 1 2 3
┌────┐
↓1   │
│2   │
│3   │
└Int8┘
      enlist (1 2 3) (4 5)
┌────┐
↓1   │
│2   │
│3   │
│4   │
│5   │
└Int8┘
      enclose (1 2 3) (4 5)
┌───────┐
↓[1,2,3]│
│[4,5]  │
└Int8───┘
      enlist enclose (1 2 3) (4 5)
┌────┐
↓1   │
│2   │
│3   │
│4   │
│5   │
└Int8┘
~~~


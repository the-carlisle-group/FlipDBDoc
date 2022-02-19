# enclose

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Create an enlcosed column from a simple column.{.purpose}

~~~
R=enclose X
~~~

X may be any column, or an array of columns of compatible type. If the structure of X is simple, R
is enclosed. If the structure of X is scalar or enclosed, the function has no effect.  If X is
partitioned, the partitions are discarded and the result enclosed. If X is an array of columns, R
is a partitioned column whose items are composed of the items of X.

See also: →[disclose], →[enlist]

## Examples

~~~
       enclose 1
┌───────┐
│1      │
└Boolean┘
       enclose 1 2 3
┌───────┐
│[1,2,3]│
└Int8───┘
       enclose (1 2 3) (4 5 6 7) (8 9)
┌─────────┐
↓[1,2,3]  │
│[4,5,6,7]│
│[8,9]    │
└Int8─────┘
       enclose enclose (1 2 3) (4 5 6 7) (8 9)
┌───────────────────┐
│[1,2,3,4,5,6,7,8,9]│
└Int8───────────────┘
       enclose enclose enclose (1 2 3) (4 5 6 7) (8 9)
┌───────────────────┐
│[1,2,3,4,5,6,7,8,9]│
└Int8───────────────┘
~~~


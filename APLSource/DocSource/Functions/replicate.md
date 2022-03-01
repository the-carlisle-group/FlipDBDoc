# replicate

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Selection{.info}

~~~
R=X replicate Y
~~~

X is a simple Integer column. Y is any column or object array. X has shape as of Y or is a scalar.

R is Y with elements, partitions or objects copied according to corresponding values in X. If the
ith item of X is n there will be n consecutive copies of the ith item of Y. If the jth item of X
is zero there will be no copies of the jth item of Y. If the kth item of X is minus m there will
be m consecutive copies of the 'type' of the kth item of Y. (That is the kth item with all
characters replaced by blanks and all numbers by zeros). X may not be negative if Y is an object array.

If X is all ones and zeros then it is a selection mask on Y.

The →[shape] of R will be the sum of the →[absoluteValue] of X.

See also: →[where]

## Examples:

~~~
      1 0 2 1 replicate 5 6 7 8
┌────┐
↓5   │
│7   │
│7   │
│8   │
└Int8┘
      1 0 1 0 replicate 5 6 7 8
┌────┐
↓5   │
│7   │
└Int8┘
~~~


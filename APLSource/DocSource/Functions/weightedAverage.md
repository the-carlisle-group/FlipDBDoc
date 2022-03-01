# weightedAverage

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

Weighted average{.purpose}

~~~
R=weightedAverage X Y
~~~

X and Y must be numeric, conformable columns.  R is the average of X weighted by Y. R is Float.

This function is equivalent to:

~~~
(sum X times Y) divide sum Y
~~~

Use the →[where] function for a non-zero weighted average of X:

~~~
weightedAverage X Y where X > 0
~~~

## Examples

~~~
      weightedAverage (1 2 3) (10 90 100)
┌─────┐
│2.45 │
└Float┘
      weightedAverage (1 2 3) (1 1 0)
┌─────┐
│1.5  │
└Float┘
      weightedAverage (0 10 20) (1 1 1)
┌─────┐
│10   │
└Float┘
      weightedAverage (0 10 20) (1 1 1) where 0 1 1
┌─────┐
│15   │
└Float┘
~~~


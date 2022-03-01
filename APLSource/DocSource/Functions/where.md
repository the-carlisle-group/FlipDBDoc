# where

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Selection{.info}

Select items{.purpose}

~~~
R=[X] where Y
~~~

X is one or more columns of any type but all of the same shape. Y is boolean of the same shape as
the columns in X. R is each column in X selected by the selection mask Y; kept where Y is 1;
discarded where Y is 0.

See also: →[replicate]

## Examples

~~~
      1 2 3 4 5 where 0 1 0 1 0
┌────┐
↓2   │
│4   │
└Int8┘
      (1 2 3) (3 5 6) where 1 0 1
 ┌────┐  ┌────┐
 ↓1   │  ↓3   │
 │3   │  │6   │
 └Int8┘  └Int8┘
      weightedAverage (0 1 2) (10 1 1)
┌─────┐
│0.25 │
└Float┘
      weightedAverage (0 1 2) (10 1 1) where 0 1 1
┌─────┐
│1.5  │
└Float┘

      where 0 0 1 0 1 1 0
┌────┐
↓2   │
│4   │
│5   │
└Int8┘

~~~


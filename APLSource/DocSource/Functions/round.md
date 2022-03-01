# round

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=[X] round Y
~~~

X and Y must be numeric. R is Y rounded to X decimal places. The default value for X is 0.

If X is 0, R is type Integer. If  X is between 1 and 9, R is type Decimal. If X is greater than 9,
R is type Float.

See also: →[bankersRound] →[roundByFactor]

## Examples

~~~
      round 1 1.2 1.23 1.234
┌────┐
↓1   │
│1   │
│1   │
│1   │
└Int8┘
      2 round 1 1.2 1.23 1.234
┌──────┐
↓1     │
│1.2   │
│1.23  │
│1.23  │
└Dec(2)┘
~~~


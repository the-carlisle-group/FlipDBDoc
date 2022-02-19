# roundByFactor

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=[X] roundByFactor Y
~~~

X and Y must be numeric. R is Y rounded to the nearest multiple of X. The default value for X is 1.

See also: →[round] →[bankersRound]

## Examples

~~~
       roundByFactor 1 1.234 9.87654
┌────┐
↓ 1  │
│ 1  │
│10  │
└Int8┘
      1 2 3  roundByFactor 1 1345.1234 9.87654
┌─────┐
↓   1 │
│1346 │
│   9 │
└Int16┘
     .25 roundByFactor .22 1.9 3.49
┌──────┐
↓0.25  │
│2.00  │
│3.50  │
└Dec(2)┘
~~~


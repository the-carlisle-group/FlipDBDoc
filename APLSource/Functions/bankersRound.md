# bankersRound

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=[X] bankersRound Y
~~~

X and Y must be numeric. R is Y rounded to X decimal places.
The default value for X is 0.

Traditional arithmetic rounding always rounds .5 away from zero.
This function uses "bankers rounding", which rounds .5 to
the nearest even value, thus sometimes rounding .5 away from zero,
sometimes towards zero.

If X is 0, R is type Integer.
If  X is between 1 and 9, R is type Decimal.
If X is greater than 9, R is type Float.

See also: →[round] →[roundByFactor]

## Examples

~~~
      bankersRound 3.4 3.5 4.5 4.6
┌────┐
↓3   │
│4   │
│4   │
│5   │
└Int8┘

~~~


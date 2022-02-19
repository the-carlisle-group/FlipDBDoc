# averageLife

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Average Life of an investment.{.purpose}

~~~
R=[X] averageLife Y
~~~

X is an optional interest rate, must be between 0 and 1, and defaults to 0. Y is a series of cash
flows.  If X is 0, Y is a series of principal cash flows.

R is the average life. R is Float.

See also: →[mortgageWAL]

## Examples

~~~
      averageLife 100 100 50 50
┌────────────────┐
│2.16666666666667│
└Float───────────┘
      0.1 averageLife 100 100 50 50
┌───────────────┐
│2.2314118629908│
└Float──────────┘
~~~


# irr

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Internal Rate of Return{.purpose}

~~~
R=[X] irr Y
~~~

* X is an initial guess (Optional; default is 0.1).
* Y is a series of cash flows.

R is the interest rate. R is Float. At least one of the cash flows must be negative and at least
one must be positive. In some cases there is more than one solution. To ensure the correct result,
use an initial guess close to the output you expect.

See also: →[npv]

## Examples

~~~
     irr -100 50 50 25
┌────────────────┐
│0.13476542104057│
└Float───────────┘
~~~


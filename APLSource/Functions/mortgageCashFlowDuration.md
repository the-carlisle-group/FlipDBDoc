# mortgageCashFlowDuration

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Compute the duration from mortgage cash flow.{.purpose}

~~~
R=Y mortgageCashFlowDuration X
~~~
R is the duration of monthly mortgage cash flow X given yield Y.
Cash flow is generally the sum of scheduled principal, scheduled interest,
and prepayments.

## Examples

~~~
     12 mortgageCashFlowDuration 1 1 1 1 1 1 1 1 1 1 1 101
┌────────────────┐
│0.94730235401829│
└Float───────────┘

~~~


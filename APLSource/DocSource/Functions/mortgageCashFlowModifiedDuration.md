# mortgageCashFlowModifiedDuration

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Compute modified duration from mortgage cash flow.{.purpose}

~~~
R=Y mortgageCashFlowModifiedDuration X
~~~
R is the modified duration of cash flow X given yield Y.
Cash flow is generall the sum of principal, interest, and prepayments.

## Examples

~~~
      12 mortgageCashFlowModifiedDuration 1 1 1 1 1 1 1 1 1 1 1 101
┌────────────────┐
│0.93792312279039│
└Float───────────┘



~~~


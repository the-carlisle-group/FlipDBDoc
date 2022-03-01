# mortgageCashFlowAverageLife

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Compute average life from a mortgage cash flow.{.purpose}

~~~
R=mortgageCashFlowAverageLife X
~~~
R is the average life in years of monthly mortgage cash flow X.
The cash flow should only be principal,
generally scheduled principal plus prepayments.

## Examples

~~~
      mortgageCashFlowAverageLife 0 0 0 0 0 0 0 0 0 0 0 100
┌─────┐
│1    │
└Float┘


~~~


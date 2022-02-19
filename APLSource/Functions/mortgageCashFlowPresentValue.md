# mortgageCashFlowPresentValue

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Compute present value of a mortgage cash flow.{.purpose}

~~~
R=Y mortgageCashFlowPresentValue X
~~~
R is the present value of the monthly cashflow X
at mortgage yield Y.

## Examples

~~~
      12 mortgageCashFlowPresentValue 10 10 10 10 1010
┌─────┐
│1000 │
└Float┘

      5 mortgageCashFlowPresentValue 10 10 10 10 1010
┌───────────────┐
│1028.8055985755│
└Float──────────┘

      18 mortgageCashFlowPresentValue 10 10 10 10 1010
┌───────────────┐
│976.08677513521│
└Float──────────┘

~~~


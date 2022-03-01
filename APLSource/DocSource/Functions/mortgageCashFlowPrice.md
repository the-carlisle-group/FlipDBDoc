# mortgageCashFlowPrice

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Compute price from mortgage cash flow.{.purpose}

~~~
R=Y B mortgageCashFlowPrice X
~~~
R is the price of cash flow X given mortgage yield Y and current balance B.

## Examples

~~~
      12 1000 mortgageCashFlowPrice 10 10 10 10 1010
┌─────┐
│100  │
└Float┘

      5 1000 mortgageCashFlowPrice 10 10 10 10 1010
┌───────────────┐
│102.88055985755│
└Float──────────┘

      15 1000 mortgageCashFlowPrice 10 10 10 10 1010
┌───────────────┐
│98.795541238857│
└Float──────────┘
~~~


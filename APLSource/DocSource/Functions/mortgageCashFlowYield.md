# mortgageCashFlowYield

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Compute yield given a price and a mortgage cashflow.{.purpose}

~~~
R=P B mortgageCashFlowYield X
~~~
R is the mortgage yield of cashflow X at price P and current balance B.
If the function fails to converge, a ¯999 or ¯888 is returned.

## Examples

~~~

      100 1000 mortgageCashFlowYield 10 10 10 10 1010
┌─────┐
│12   │
└Float┘

      101 1000 mortgageCashFlowYield 10 10 10 10 1010
┌───────────────┐
│9.5424430231541│
└Float──────────┘

      99 1000 mortgageCashFlowYield 10 10 10 10 1010
┌──────────────┐
│14.48762113093│
└Float─────────┘
~~~

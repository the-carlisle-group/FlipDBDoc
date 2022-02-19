# mortgageCashFlowConvexity

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Compute convexity from mortgage cash flow.{.purpose}

~~~
R=Y mortgageCashFlowConvexity X
~~~
R is the convexity of cash flow X given yield Y.

## Examples

~~~
    {⍵.Display 0}12 mortgageCashFlowConvexity 1 1 1 1 1 1 1 1 1 1 1 101
┌────────────────┐
│0.98767539196594│
└Float───────────┘

~~~


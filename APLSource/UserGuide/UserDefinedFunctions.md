# User Defined Functions

An expression set may be used to implement a user-defined function. Consider the following expression set,
created and saved as `/Objects/ComputeNewBalance`:

|Name|Expression|
|----|----------|
|`Interest`|`Balance*Rate/1200`|
|`Principal`|`PandI-Interest`|
|`NewBalance`|`Balance-Principal`|

The →[*.evaluateExpressionSet] function executes the expression set as a user-defined
function:

|Name|Expression|
|----|----------|
|`NextBalance` |`evaluateExpressionSet '/Objects/ComputeNewBalance'`

The result of `evaluateExpressionSet` is the value of the last expression
in the expression set. Thus the name associated with the last expression
(NewBalance in this expample) is immaterial. In the case above,
we have assigned the result to the name of our choosing, `NextBalance`.

Note also there is no need to hide and names using a Hide column, as the last expression
is the only expression returned.

The evaluateExpressionSet function accepts an optional left argument.
When this argument is omitted, as in the above example,
the expressions are evaluated in the calling context.
All the names in the calling context are visible inside the expression set
and may be referenced. All assigned names are stritly local and do not affect
the calling environment.

A more functional approach may be taken by passing in a propertyspace as the left
argument:

|Name|Expression|Hide|
|----|----------|----|
|`Input`|`newPropertySpace ''`|1|
|`Input.Balance`|`CurrentBalance`|1|
|`Input.Rate`|`Coupon`|1|
|`Input.PandI`|`MonthlyPayment`|1|
|`NextBalance` |`Input evaluateExpressionSet '/Objects/ComputeNewBalance'`|0|

In this case, the calling context is not available. The only names the expression set
can see are those provided in the left argument.

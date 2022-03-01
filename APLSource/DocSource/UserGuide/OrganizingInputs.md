# Organizing Inputs

Just as any large program, function, or script benefits from being broken into smaller
components, so too large expression sets benefit by refactoring
them into smaller expression sets. Expression sets may be used to organize and
modularize code using the →[*.evaluateExpressionSet] function.

Consider the →[*.mortgageCashFlow] function, which takes dozens of named inputs
assembled in a single propertyspace. We might initially design a single
expression set to assemble the inputs, run the cashflows, and then pick some outputs:

|Name|Expression|Hide|
|----|----------|----|
|`LoanNumber`|`LoanID`|`0`|
|`BorrowerName`|`BorrowerName`|`0`|
|`CashFlowIn`|`newPropertySpace ''`|`1`|
|`CashFlowIn.Balance`|`CurrentBalance`|`1`|
|`CashFlowIn.Rate`|`Coupon`|`1`|
|`CashFlowIn.Term`|`OriginalTerm - Age`|`1`|
|`CashFlowIn.StatedYield`|`3.125`|`1`|
|`...`|`...`|`1`|
|`CashFlowOut`|`mortgageCashlow CashFlowIn`|`1`|
|`Price`|`CashFlowOut.ComputedPrice`|`0`|
|`InterestOverLife`|`sum CashFlowOut.CashFlow.Interest`|`0`|


It is useful to package the marshalling of
all of these inputs into it own expression set, separating
it from the main outputs of the expression set:

|Name|Expression|Hide|
|----|----------|----|
|`LoanNumber`|`LoanID`|`0`|
|`BorrowerName`|`BorrowerName`|`0`|
|`CashFlowIn`|`evaluateExpressionSet '/Objects/CashFlowInputs'`|`1`|
|`CashFlowOut`|`mortgageCashFlow CashFlowIn`|`1`|
|`Price`|`CashFlowOut.ComputedPrice`|`0`|
|`InterestOverLife`|`sum CashFlowOut.CashFlow.Interest`|`0`|

Where `Objects/CashFlowInputs` is defined as:

|Name|Expression|
|----|----------|
|`Arg`|`newPropertySpace ''`|
|`Arg.Balance`|`CurrentBalance`|
|`Arg.Rate`|`Coupon`|
|`Arg.Term`|`OriginalTerm - Age`|
|`Arg.StatedYield`|`3.125`|
|`...`|`...`|
|`I`|`Arg`|

Note that evaluateExpressionSet returns the value of the last expression,
in this case the propertyspace named `Arg`. Thus there is need to provide
a Hide column. In addition, the name `I` on the last line is immaterial.


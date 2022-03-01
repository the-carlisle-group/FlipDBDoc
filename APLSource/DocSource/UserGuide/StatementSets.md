# Statement Sets

An expression set where all of the expressions are boolean valued is termed a
**statement set**. Statement sets may be executed using the →[*.evaluateStatementSet]
function. Note that Statement sets may be used directly as the →[*.GroupBy]
property of a query.

Consider the statement set created and named as `/Objects/DataProblems`:

|Name|Expression|
|----|----------|
|`MissingLTV`|`LTV eq 0`|
|`InvaldState`|`not okState State`|
|`MissingMargin`|`(RateType in 'A') and (Margin eq 0)`|

This may be executed analyzed as:

|Name|Expression|Hide|
|----|----------|----|
|`DataProblems`|`evaluateStatementSet '/Objects/DataProblems'`|`1`|
|`None`|`none DataProblems`|`0`|
|`Any`|`any DataProblems`|`0`|
|`All`|`all DataProblens`|`0`|

The result DataProblems is partiioned boolan column, with, in this case,
3 values in each partition. The aggregate function →[*none] returns a 1
for each row that has no problems. The aggregate runction →[*.any] returns a
1 for each row that has at least one problem. The aggregate function →[*.all] returns
a 1 for each row that has all three problems.


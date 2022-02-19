 # Introduction

An expression set is an ordered set of expressions.
It is a DataTable with one column named Expression and
optionally other system-recognized column names.
An expression set may have additional, user-defined columns as well.

Expression sets form the basis of almost all of FlipDB.
For example, the Where property of a query is an expression set:

|Expression|
|----------|
|`Balance gt 100000`|
|`State in 'NY,NJ,CT'`|
|`PropertType in 'SingleFamily'`|

And the Select property of a query is an expression set:

|Name|Expression|
|----|----------|
|`Count`|`count LoanNumber`|
|`Balance`|`sum CurrentBalance`|
|`WAC`|`weightedAverage Rate CurrentBalance`|

Note that the Where property is composed of unnamed expressions,
while the Select property has named expressions.
Thus expression sets may have named or unnamed expressions.

FlipDB recognizes a small set of reserved column names in expression sets:

|Column Name|Description|
|-----------|-----------|
|Name| The name of the expression. This must be a valid FlipDB name.'|
|Expression| The expression|
|Value| An associated expression, used in case statements where Expression is boolean.|
|DisplayName| A named used for display purposes. May be any string.|
|Hide| A boolean flag indicating that the Name should not be returned in the result, if applicable.'|
|Inactive| A boolean flag indicating that the expression should not be evaluated.|

Users may add additional columns to an expression set which are ignored by FlipDB.
Users typically add a comment column.

Expression sets may be thought of as structured programs or functions or scripts.
As such, an expression set can call and evaluate an expression set. This may be
done in a number of different ways, using one of the following  functions:
→[*.evaluateExpressionSet], →[*.evaluateCaseStatement], and →[*.evaluateStatementSet].
Each of these functions is discussed in detail in the following user guide sections.


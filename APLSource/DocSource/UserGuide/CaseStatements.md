# Case Statements

Expression sets may be used to implement case statements which are
executed by the →[*.evaluateCaseStatement] function. When the expressions
in the expression are all boolean valued, the expression set is called a
a **statement set**. The `evaluateCaseStatement` function operates on statement sets.

Note: Statement sets may be used directly as the →[*.GroupBy] property of a query.

Consider the following statement set `/Objects/ComputeRegion`:

|Name|Expression|
|----|----------|
|`East`|`State in'NY,NJ,CT'`|
|`West`|`State in 'CA,WA,ID,MT'`|
|`South`|`State in 'FL,GA,AL,MS,LA'`|

This may be evaluated as:

|Name|Expression|
|----|----------|
|`SalesRegion`|` evaluateCaseStatement '/Objects/ComputeRegion'`|

SalesRegion will be a simple column with the values East, West, and South, one value
for each row in the table.

It is often the case however that the desired rusult per statement is variable
and not constant. If a Value column containing expressions is provided,
it is used to compute the result rather than the name column. For example:

|Expression|Value|
|----------|-----|
|`OriginalTerm in 120 240 360`|`OriginalTerm`|
|`OriginalTerm eq 0`|`MaturityDate monthsDifference OriginationDate`|
|`OriginalTerm eq OriginalTerm`|`360`|

In this example, the last expression is a catchall, so that any row not caught in
the first two statements will be assigned the value 360.

Note that when a Value column is provided, the result may be numeric or date or
any type, provided all of the types of each Value expression are compatible.

The default behavior of evaluateCaseStatement is to enforce mutually exclusive groups.
That is, if a row is selected by a statement, it is removed from consideration in
subsequent expressions. This may be overriden by providing a 1 as the left argument,
to allow overlapping groups. When this is the case, the result of the function
is a partioned column, containing zero or more values for each row. Consider the
statement set `/Objects/PriceFactors`:

|Name|Expression|Value|
|----|----------|-----|
|California|`State in 'CA'`|`.25`|
|Condo|`PropertyType in 'Condo'`|`.125`|
|HighLTV|`LTV gt 95`|`1.25`|

Using a left argument of 1, we can execute this and compute various values from the result, inlcuding the
number of hits, the sum the factors, and a comma delimited list of factor that apply:

|Name|Expression|Hide|
|----|----------|----|
|`Factors`|`1 evaluateCaseStatement 'Objects/PriceFactors'`|1
|`FactorCount`|`count Factors`|0
|`TotalFactors`|`sum Factors`|0
|`FactorList`|`toCSV Factors`|0


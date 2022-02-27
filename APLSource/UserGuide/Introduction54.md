# Introduction

A FlipDB query may be thought of as a template for a highly structured,
highly parameterized program.

## DataSource

A query begins with the specification of a data source: the name of a database
and table. This is known as the starting table of the query. All unqualified names
specified in the query are assumed to be columns in this table.

## Computed Columns
Once the data source is specified, the query continues by evaluating the →[*.ComputedColumns] property. This provides
a mechanism to define new columns, values and constants that may be referenced
later in the query, in addition to all the columns of the table. Consider:

|Name|Expression|
|----|----------|
|`Interest`|`Balance*Rate/1200`|
|`AsOfDate`|`2020/12/31`|
|`TotalBalance`|`sum CurrentBalance`|

The computed columns property provides two major benefits. First, it provides a place
to define a name and expression once, and have it available throughout the query.
For example, the Interest column above may be referenced in the Where
clause and the GroupBy clause. This eliminates duplication and increases performance and maintainability.

Second, computed columns are evaluated before the Where and
GroupBy clause and thus operate on the entire, ungrouped table. For example, if there is
a subsequent where clause that selects, say, only fixed rate mortages, the
selected total balance of the fixed rate loans may easily be compared to
the total balance of the entire table.  Note however that computed columns that are simple
(not aggregated or enclosed) are treated like regular columns of the table and subsequently
filtered by the Where and WhereNot clauses, and partitioned by the GroupBy clause.

## Where Clause
After the computed columns are evaluated, FlipDB applies the →[*.Where] property.
The Where property specifies one or more statements, which are logically and-ed
together to filter the table rows. Computed columns are filtered as well (Note that
scalar or enclosed columns are not filtered). For example:

|Expression|
|----------|
|`State in 'NY,NJ,CT'`|
|`PropertyType in 'Condo'`|
|`LTV le 80`|

This is equivalent to writing:

|Expression|
|----------|
|`(State in 'NY,NJ,CT') and (PropertyType in 'Condo') and (LTV le 80)`|

There are many benefits to breaking up long statements into multiple short statements.
First, short statements with fewer parentheses are much easier to maintain and read.
Second is better performance. Third, and perhaps most importantly, multiple
statements can be analyzed independently. The query may be traced step by step,
to see the net number of rows selected with each expression.  In addition the entire
Where clause can be redirected as the GroupBy clause of a new query, or analyzed using
the →[*.evaluateStatementSet] function.

The →[*.WhereQuota] property specifies a hard limit for the number of rows to
select. It is applied after the Where and WhereNot statements have been applied.

## Where Not Clause
Sometimes it is more usefule to describe the rows that are to be removed from a query
rather than the rows to be selected. (Sometimes both techniques together are useful.)
The →[*.WhereNot] property specifies rows to remove from the query.
Multiple statements are logically or-ed together. For example:

|Expression|
|----------|
|`Balance eq 0`|
|`not okState State`|
|`LTV eq 0`|

This is equivalent to writing:

|Expression|
|----------|
|`(Balance eq 0') or (not okState 'State') or (LTV eq 80)`|

Multiple, small where not statements have all the advantages of multiple where statements
listed above.

In general, when there are multiple Where statements (W1 to Wn) and
multiple WhereNot statements (WN1 to WNn) they are combined as follows
to select the rows of the table:

|Expression|
|----------|
|`(W1 and W1 and W3...Wn) and not (WN1 or WN2 or WN3 ...WNn)`|

## GroupBy Clause
Once the table's rows have been filtered by the Where and WhereNot statements,
the →[*.GroupBy] property may group the query. For example:

|Name|Expression|
|----|----------|
|`GeographicDistribution`|`State`|

Multiple breaks may be specified. In addition, numeric and date values are often ranged:

|Name|Expression|
|----|----------|
|`GeographicDistribution`|`State`|
|`OriginationYear`|`12 range OriginationDate`|
|`MorgageRate`|`0 10 .5 range Rate`|

Note that the Name specified in the GroupBy clause is available for further
analysis in the Select clause. That is, in the above example, the name
GeogrpahicDistribtion is available and contains the unique list of states. In
addition of course, the State column is available and contains the partitioned
list of states - not particulary useful in this case as each partition contains
the exact same values. However in the case of the ranged numeric columns, like Coupon
above, the grouped data will may contain multiple values.

> Warning: If you use the same name for a grouped result as the original column name,
the original column data will not be accessible in the Select clause. THis has implications
for ranged numeric or date data. For example, if you use:
if you use:

|Name|Expression|
|----|----------|
|`Rate`|`0 10 .5 range Rate`|

Then you will NOT be able to use Rate to computed a weighted average coupon.
Use different names for your groups.

## The Select Clause
For an ungrouped query the select property (also known as the columns property)
in its simplest form is a list of column names or expressions to return:

|Name|Expression|
|----|----------|
|`LoanNumber`|`LoanNumber`|
|`BorrowerName`|`FirstName catenate ' ' catenate LastName`|
|`Address`|`PropertyAddress`|
|`MaturityDate`|`MaturityDate`|

For a grouped queries the select property (also known as the the measures property)
is usually a list of aggregate expressions:

|Name|Expression|
|----|----------|
|`Count`|`count LoanNumber`|
|`Balance`|`sum CurrentBalance`|
|`WAC`|`weightedAverage Rate CurrentBalance`|

This however is just the start. Whether grouped or ungrouped, the select
property may be thought of as a program or function,
and every name that is computed may be referenced in subsequent expressions.
For example, here we compute the interest and then use that value to compute
the principal portion of the payment:

|Name|Expression|
|----|----------|
|`LoanNumber`|`LoanNumber`|
|`Balance`|`CurrentBalance`|
|`Payment`|`MonthlyPayment`|
|`Interest`|`CurrentBalance*Rate/1200`|
|`Principal`|`MonthylyPayment - Interest`|

Adding a Hide column to the expression set provides a way to
compute temporary values that we do not want to return in the result of the query:

|Name|Expression|Hide|
|----|----------|----|
|`LoanNumber`|`LoanNumber`|`0`|
|`Balance`|`CurrentBalance`|`0`|
|`Payment`|`MonthlyPayment`|`0`|
|`Interest`|`CurrentBalance*Rate/1200`|`1`|
|`Principal`|`MonthylyPayment - Interest`|`0`|

## OrderBy Clause
After the Select clause is applied, the →[*.OrderBy] clause sorts the results. The
Name column here referes to names specified in the Select clause:

|Name|Direction|
|----|---------|
|Balance|Down|

## Having Clause
After the results are sorted, the →[*.Having] clause applies a final filter.
The Having clause specifies statements that operate on names specified in the
Select clause:

|Expression|
|----------|
|Balance gt 5000000|

Multiple Having statements are and-ed together just like Where statements.

The →[*.HavingQuota] property specifies a specfic number of rows to keep.

The →[*.RollUp] property, applicable only to grouped queries,
specifies that if any groups are exluded from the
result by the Having or HavingQuota property,
they are rolled up into a single "Other" category.


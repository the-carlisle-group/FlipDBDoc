# Grabbing Data From Anywhere

FlipDB is a relational database management system that
provides for the formal definition of primary and foreign keys and
a unique and rich →[*.User_Guide.Joins|Join] syntax for pulling in data from other tables.

However, in much ad-hoc analysis, a formal database with foreign keys and referential integerity
is simply not available. There is simply data in various databases and tables,
and in previously computed query results that we need to access and incorporate
into the current query. FlipDB provides an easy way to directly access all of this data.

The →[*.getColumn] function will directly access a column located anywhere: in the
same table, another table in the same database, or in any other database:

|Name|Expression|
|----|----------|
|`TotalBalance`|`sum getColumn 'Loans.Balance'`|

Note that a column accessed this way is not affected by the Where clause. This
provides one technique reaching out of the current context.

The →[*.getTable] function will get an entire table from the current database
or any other database, materializing it as a datatable. It is usually conventient to apply the →[*.transpose] function to convert the
datatable to a propertyspace, making it easy to dot into and access the columns:

|Name|Expression|
|----|----------|
|`StateInfo`|`transpose getTable 'State'`|
|`MTM`|`StateInfo.MonthsToForeclose`|

Data from previously executed queries in a Report may be direclty accessed
as propertyspaces in the Globals property space:

|Name|Expression|
|----|----------|
|`UniqueStates`|`Globals.GeographicDistribution.State`|

Values accessed in any of these ways will generally be _non-conforming_. That is, there is no
reason to think that a column from another table or query result will line up in any
way with the current query. The dimensions will be different. Sometimes this is
acceptable. For example if we are simply checking for elements
in a list in a Where clause, there is no need for the data to be conformable:

|Expression|
|----------|
|ZipCode in getcolumn 'HurricaneAffected.Zipcode'

However, many times we need to make the data conform is some way.
There are three common techniques for accomplishing this. First, we may **aggregate**.
For example:

|Name|Expression|
|----|----------|
|`AnnualTarget`|`sum getColumn 'MonthlyTargets.Amount`'

Second, we may **enclose**. For example, we may have a table that contains time series
data for, say, default rates. The table is 360 rows long, and each column contains a scenario,
If we have 1000 mortgages in a main table we can enclose the time series column
so that the entire set of 360 values is applied to each mortgage:

|Name|Expression|
|----|----------|
|`DRS`|`transpose getTable 'DefaultRateScenarios'`|
|`CFI`|`newPropertySpace ''`|
|`CFI.Balance`|`CurrentBalance`|
|`CFI.Rate`|`Rate`|
|`CFI.DefaultRateScenario`|`enclose DRS.Scenario1`|

Third, we may perform a **lookup**, to line-up the data in a datatable or propertyspace
with the current query. Consider a database table named `ForeclosureInfo` that
contains 50 rows, one for each state:

|State|Months|LegalFees|
|-----|------|--------|
|NY   |24    |  2,200 |
|CA   |18    |  3,400 |
|SD   |2     |    375 |
|FL   |7     |    975 |
|...  |...   |...     |

Consider further that the current query is a simple loan list:

|LoanID|PropertyState|
|------|-------------
|101|CA          |
|102|CA          |
|103|NY          |
|104|FL          |
|...|...         |

The →[*.conform] function can take this table and the name of one
or more columns that define a lookup key, and conform it to the current
query dimensions:

|Name|Expression|
|----|----------|
|`ForcInfo`|`PropertyState conform 'ForclosureInfo' 'State'`|

It is convenient to →[*.transpose] the result to yield a propertyspace,
to subsequently access the columns. Thus a simple loan list might be defined as:

|Name|Expression|
|-----|---------|
|`ForcInfo`|`transpose PropertyState conform 'ForclosureInfo' 'State'`|
|`LoanID`|`LoanNumber`|
|`PropertyState`|`PropertyState`|
|`MonthsToForclose`|`ForcInfo.Months`|
|`LegalFees`|`ForcInfo.LegalFees`|

And yield:

|LoanID|PropertyState|MonthsToForeclosure|LegalFees|
|------|-------------|-----------------|---------|
|101|CA|18| 3,400|
|102|CA|18| 3,400|
|103|NY|24| 2,200|
|104|FL|7 |  975|
|...|...|...|...|

The conform function works on an entire table. This is generally fine for
small lookup style tables, or previously computed grouped query results.
For very large tables with millions of rows and a large number of columns,
it can be inefficient. The →[*.indexOf] (and →[*.lookup]) and →[*.index] functions
may be used to work on specific individual columns of large tables
in a more efficient, if more verbose, way:

|Name|Expression|
|----|----------|
|`LoanID`|`getColumn 'Tab1.LoanID'`|
|`K`|`Col1 indexOf LoanNumber`|
|`Balance`|`K index getColumn 'Tab1.Balance'`|




# Introduction to Reporting

FlipDB reports can replace extremely complex spreadsheets with a vastly simpler,
linear design; one that is much easier to write, read, maintain and audit.
A FlipDB report may be thought of as a highly parameterized, highly structured program.

A FlipDB report begins with the specification of the →[*.Properties.DataSource] property,
which specifies a default database and table. This property is discussed at length
in the →[DataSource|next section].

After the DataSource is specified, a report continues with the
optional specification of global variables via the →[*.Globals] property,
an expression set evaluated in the context of the DataSource.
Typical globals might include constants like an as-of date, or a company name,
but it may also include any arbitrary expression. For example:

 | Name | Expression
 |------ | -----------
 | `AsOfDate`   | `2020/12/31`
 | `Company`    | `'Carlisle Group'`
 | `WestRegion` | `'MT,CO,ID,WY'`
 | `GrandTotal` | `sum Balance`

One of the benefits of an expression like `GrandTotal` above is that it operates outside
the scope of where clauses in subsequent queries.

After the DataSource and Globals are specified, a FlipDB report
continues with the specification of one or more →[*.Objects.Query|Queries].
A query may access globals by using the Globals propertypace.
(Note that Globals is thus a reserved word within a report.) For example, a
query might include the following where clause:

 | Expression |
 |------ |
 | `State in Globals.WestRegion`|

The result of the query itself is automatically injected into the Globals propertyspace,
as a sub propertyspace, using the the name of the query. Thus any query can
reference values computed in a previous query. For example, consider a query
named `GeoDist` that produces the following:

| State|Count|Balance|
| -----|-----|-----------|
| NY   |1,125| 23,423,345|
| NJ   |   56|  1,435,354|
| CT   |  314| 10,454,198|
| Other|   17|    565,356|

After this query executes, a propertyspace named GeoDist will be accessible
uner the Globals propertyspace. In a subsequent query the following expression
computes the total balance for New York and New Jersey from the result of
the GeoDist query:

~~~
   sum Globals.GeoDist.Balance where Globals.GeoDistState in 'NY,NJ'
~~~

In addition, a query may inject arbitrary values into the Globals propertyspace
to be used by subsequent queries. Consider this select clause of an ungrouped query:

 | Name | Expression | Hide|
 |--------------------|------------------------|--|
 | `LoanID`           | `LoanID`               | 0|
 | `Balance`          | `Balance`              | 0|
 | `InterestRate`     | `RATE`                 | 0|
 | `MonthlyInterest`  | `Balance * Rate / 1200`| 0|
 | `Globals.TotalInt` | `sum MonthlyInterest`  | 1|

Here the variable `TotalInt` is assigned into the Globals space for later use.
The Hide column is used to prevent TotalInt being retured as part of the query.

In this way, every value computed by a query can be accessed and used for further
analysis in subsequent queries.


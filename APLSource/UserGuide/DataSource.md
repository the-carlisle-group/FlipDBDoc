# The DataSource Property

The DataSource property of a report is itself a query that specifies a default table for other
queries in the content of the report. In addition, the DataSource query may limit the rows in this
table by applying Where and WhereNot clauses.

When a query is embedded in a report, one or more of the DatabaseName, TableName, and AsOf
properties may be left unspecified and will default to the corresponding values of the DataSource
property. If all three of these properties are left unspecified, the embedded query is said to be
"dependent". The default table is the starting point of a dependent query, which may further limit
the rows by applying its own Where and WhereNot clauses.

If all three properties are specified in an embedded query, the query is said to be "independent".
This is useful for cross-database reporting: reaching into another database and table.

If one or two of these properties are specified in the embedded query and the remainder default to
values in the DataSource property, the query is said to be "semi-dependent." The most useful
semi-dependent queries are those where the TableName is explicitly set in the query, but the
DatabaseName property defaults to the DataSource.

The Where and WhereNot clauses of the DataSource query only effect dependent queries; they have no
effect on independent or semi-dependent queries.

Dependent, semi-dependent and independent queries may all be used in the same report. In general,
the most commonly used table should be selected as the default table and specified in the
DataSource property. As many queries as possible should be defined as dependent. A report that
contains only dependent queries may easily be pointed at another table with a similarly schema in
the same database or in a different database. If a query must start with a different table in the
same database, it will be semi-dependent. In this case, the report may easily be pointed to a
different database with a similarly schema. And any query that must look outside the default
database will be independent. For maximum portability and ease of maintenance, queries should be
dependent if possible, independent if necessary, and independent as a last resort.


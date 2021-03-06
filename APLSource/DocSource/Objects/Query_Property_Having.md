# Having Property

Applies to:{.prefix}

→[##.##.Query]{.info}

The Having property specifies one or more Boolean expressions used to filter the number of rows
in the query result at the end of a query, after grouping is applied and after all column
expressions are evaluated. This is in contrast to the Where property, which is applied at the
beginning of the execution of a query.

The Having property is formally a DataTable with an Expression column (a Boolean expression set),
and may be specified as such, but it may also conveniently be specified as a simple string. The
helper method →[##.MethodList.AddHaving] may be used to incrementally add a having expression.
Like the Where property, multiple having expression are "and-ed" together. All names referred to in
having expression must exist in the result of the query. In other words, while a where expression
may refer to any column in the starting table, a having expression may only refer to Select
property names.

If no grouping is applied, and if there are no hybrid or summary functions in the column
expressions, then the Having property has the same effect as an additional clause in the →[Where] property.


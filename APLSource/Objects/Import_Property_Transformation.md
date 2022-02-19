# Transformation Property

Applies to:{.prefix}

→[##.##.Import]{.info}

This property specifies how the source data is transformed before creating or updating the target
table. It is an expression set (a DataTable with "Name" and "Expression" columns), where the
"Name" column represents the target name, and the "Expression" column represents the source expression.

Essentially it is the Select property of a query applied to the source table, where the result of
the query is used to create or update the target table.


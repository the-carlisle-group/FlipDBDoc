# TableName Property

Applies to:{.prefix}

→[##.##.Query]{.info}

This property specifies the name of the Table to which the query will be applied. The existence of
the table is not checked when setting the property, but only upon running the
→[*.Query.MethodList.Execute|Query.Execute] method.

When a query is embedded in a report this property may be left blank to indicate that its value
should be inherited from the →[*.Report.Properties.DataSource] property of the report.


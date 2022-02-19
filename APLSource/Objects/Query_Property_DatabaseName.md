# DatabaseName Property

Applies to:{.prefix}

→[##.##.Query]{.info}

This property specifies the name of the Database to which the query will be applied. The existence
of the database is not checked when setting the property, but only upon running the
→[##.MethodList.Execute] method.

When a query is embedded in a report this property may be left blank to indicate that its value
should be inherited from the →[*.Report.Properties.DataSource] property.


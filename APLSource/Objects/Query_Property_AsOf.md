# AsOf Property

Applies to:{.prefix}

→[##.##.Query]{.info}

The AsOf property specifies what state of the database a query should operate on. Normally it is
set to 0, which indicates the current or latest state of the database. If a DateTime is specified
the query is applied to the database as it appeared at that time. If a negative integer N is
specified, the database is rolled back M transactions, where M is the absolute value of N (for
purposes of the query only).

The AsOf property may also be set to 1. In this case the query operates on the complete history of
a table, and including deleted and inactive rows. Duplicate keys values may result.

The AsOf property may also be set to 2. This value indicates that the AsOf property should
inherit its value from the Report.DataSource property should the query be embedded in a report.
This is the default value when a new query is added to a report using the GUI. This value has no
effect in a stand-alone query, and will behave as the value 0.

This property may be overridden when running a query by providing a suitable argument to the
→[##.MethodList.Execute] method. This argument may be any of the values specified above except 2.
It may also be void, to indicate the actual value of the AsOf property is to be used and not overridden.


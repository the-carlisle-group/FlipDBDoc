# AddGroupBy Method

Applies to:{.prefix}

→[##.##.Query]{.info}

This method is used to incrementally add a grouping specification to the GroupBy property of a Query.

The argument is composed of two items:

|-|-|
|1|Name|String|
|2|Expression|String|

The result is composed of one item:

|-|-|
|1|GroupBy|DataTable|

The Name must be a valid column name. The Expression must be a valid expression or column name that
results in a simple column. See the →[##.Properties.GroupBy] property for details.


# AddColumn Method

Applies to:{.prefix}

→[##.##.Query]{.info}

This method incrementally adds a column to the →[##.Properties.Select] property, which is then
returned as the result.

The argument is composed of one, two or three items:

|-|-|
|1|Name|String|
|2|Expression|String|
|3|Hide|Boolean|

The result is composed of one item:

|-|-|
|1|Select|DataTable|

The Hide item defaults to 0. The Expression item defaults to the value of the Name item. See the
Select property for a full discussion of usage.


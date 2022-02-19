# Globals Property

Applies to:{.prefix}

→[##.##.Report]{.info}

The Globals property is an expression set evaluated in the context of the
database and table specified in the →[DataSource] property.

All queries in the →[Content] of the report may refer to values
in the **Globals** propertyspace.

In addition the Globals propertyspace mayt be used to store and pass values
from one query to another in a report.

Note that global values are not effected during the execution of the query
by by any properties in the query. This is in contrast to ComputedColumns,
which are filtered the Where and WhereNot clauses, and grouped by the GroupBy clause.




# WhereQuota Property

Applies to:{.prefix}

→[##.##.Query]{.info}

The WhereQuota property specifies an integer that caps the number of rows identified by the
→[Where] property. The default value is 0, which indicates that no quota is in effect.

A positive value M indicates that at most M rows should be selected, starting from the “oldest”.  A
negative value N will select at most N rows, starting from “newest” or most recently added or
updated rows. (Rows are naturally ordered in tables by time of update, with older rows at the top
of the table, and newer rows at the bottom.)

The quota is applied immediately after the Where clause is evaluated, and before any grouping.

To limit the number of rows returned after grouping, see the →[HavingQuota] property.


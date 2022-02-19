# KeepEmptyGroups Property

Applies to:{.prefix}

→[##.##.Query]{.info}

This property controls the result of grouped queries, and may be set to 0 or 1 (the default).  It
has no effect on non-grouped queries.

Empty groups can occur in four cases: when grouping on a foreign key, when grouping on a column
that is the result of the →[*.range] function, when grouping by a boolean expression set, and
finally when grouping by multiple columns. If KeepEmptyGroups is 0, then empty groups are
discarded from the result, otherwise empty groups are kept in the result.

If a query is grouped by a column that is the result of the range function and KeepEmptyGroups is
1, and the upper and lower break points of the range have been chosen to include all the data,
then there will be exactly N-1 rows in the query result, where N is the number of break points.
Otherwise there will be N or N+1 rows, depending on whether or not data exists below the lowest
break point and or above the highest break point.


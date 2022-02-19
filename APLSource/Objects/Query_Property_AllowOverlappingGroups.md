# AllowOverlappingGroups Property

Applies to:{.prefix}

→[##.##.Query]{.info}

The AllowOverlappingGroups property specifies whether or not groups should be mutually exclusive.
Overlapping groups may only occur when grouping by a boolean expression set, so this property has
no effect when grouping by the unique values of columns.

If AllowOverlappingGroups is set to 0 (the default), then FlipDB enforces mutually exclusive
groups by  removing rows for consideration that fall into any preceding group. Thus the ordering
of a boolean expression set is important.


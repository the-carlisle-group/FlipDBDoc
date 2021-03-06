# LookupKey Property

Applies to:{.prefix}

→[##.##.Import]{.info}

This property specifies the column names used to look up source rows in the target table. There is
no requirement that this property represents a formal primary, alternate, or foreign key; it is
simply used to determine how rows should match up for the import. This property is only relevant
when the target table already exists. If, based on this key, there are duplicate source rows, only
the first row is used, and the duplicate source rows are discarded on the import. If, based in
this key, there are duplicate target rows, then all of the target rows that correspond to a source
row will be updated by that source row.

If the LookupKey property is empty, the default value, then no rows are considerd to match, and all
source rows are considered "new", and will be inserted into the target table.


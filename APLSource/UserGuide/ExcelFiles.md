# Excel Files

Excel files are files produced by Microsoft Excel. They usually have an "xls" or "xlsx" extension.
FlipDB assumes that the entire used range of the active sheet is to be imported. This range may or
may not have column names in the first row. The HasHeader property indicates whether or not the
first row contains field names.

FlipDB makes no assumptions about the data type of an Excel column, and imports all columns as Char
columns. Columns must be explicitly converted to the appropriate FlipDB data type by applying a
casting function like toInteger or toDate.


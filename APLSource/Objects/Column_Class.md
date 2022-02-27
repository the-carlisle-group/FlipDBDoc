# Column Object

## Purpose

This object represents a column in a [Table]() object.

## Notes

Column objects are returned as the result of the
[CreateColumn]() and [GetColumn]() methods.

The [Name]() property returns the name of the column and is read-only.
To rename a column, use the RenameColumn method of the parent table object.

The [Type]() property returns the data type of the column and is read-only.
The [ChangeType] method changes the type of the column.

The [SetForeignKey]() method specifies that the column should be a foreign key,
referencing the primary key of another table.

The Dereference method removes a foreign key specification,
disassociating the column from the primary key of another table.

If the column is specified as a foreign key, the RefersTo property returns the name of
the foreign table.

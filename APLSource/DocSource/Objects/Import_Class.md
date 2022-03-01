# Import Object

## Purpose 

The Import object provides functionality to import a variety of data sources into FlipDB tables.

## Description

There is only one method, [Execute](), which imports the data and creates a new table
or updates an existing table, which it returns as a result.
The SourceType property specifies the type of the DataSouce, and may be "CSV", "Excel"
"FixedWidth", "dBase", "CAS", "ADO", "ODBC", or "FlipDB"
Some properties apply to all SourceTypes, while other
properties only apply to one or more specific SourceTypes:

|SourceType | Applicable Properties |
|:-|:-|
|All | SourceName,TargetDatabaseName, TargetTableName ReferenceDatabaseName, ReferenceTableName, Transformation PrimaryKey, LookupKey, AddNewColumns, AddNewRows, UpdateExistingRows |
|CSV | HasHeader, Delimiter, TextQualifier, TreatConsecutiveAsOne, StartRow |
|Excel | HasHeader |
|dBase | [none] |
|CAS | [none] |
|FixedWidth | RecordLength, FixedWidthLayout, StartRow |
|ADO | Provider ,ConnectionString, CommandText |
|ODBC | ConnectionString, CommandText |
|FlipDB | [none] |

The "source specific properties" are the minimum necessary to
construct a FlipDB table from the data source. With the Execute method, FlipDB creates a
temporary table using these properties.
It then applies the Transformation property to this
table, which may change column names, modify data types, map column values,
or even compute new columns.
The transformation property is effectively the Select property
of a Query which is applied to the temporary table to produce a new, transformed, table.

The TargetDatabaseName and TargetTableName specify the target table,
which may or may not exist.

Certain properties are only applicable if the target table exists.
The LookupKey property specifies one or more column names used to compare
rows in the transformed table to the target table.
The AddNewRows property specifies whether or not new rows, as defined by the LookupKey
are to be added to the target table.
The UpdateExistingRows property specifies whether or not existing rows, as defined by the LookupKey
are to be updated in the target table.
The AddNewColumn property specifies whether or not new columns
are to be added to the target table.

Certain other properties are only applicable if the target table does not exists.
The PrimaryKey property specifies the name column of the transformed table to
be specified as the primary key.
The ReferenceDatabaseName and ReferenceTableName properties specify
the table that should be used as a guide to importing. These properties
are only used in the Import Wizard to give visual feedback when importing,
and have no effect when using the Execute method in a script.


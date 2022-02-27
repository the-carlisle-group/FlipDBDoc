# DataColumn Object

## Purpsoe 

Represents a column value.

## Description

The DataColumn is the basic data object of FlipDB, and is like
an array. A DataColumn is immutable and has no properties or methods.
FlipDB functions take DataColumns as input and return
DataColumns as results.

DataColumns have structure.
A DataColumn may be a single item (Scalar),
an array of single items (Simple),
or an array where each element is a set of items (Partitioned).

DataColumns also have type. A DataColumn may be
Char, Boolean, Integer, Decimal, Float, Date, DateTime,
Time, or Blob

A DataTable contains an array of DataColumns.
Individual DataColumns may be extracted from DataTables
using the →[*.DataTable.MethodList.GetColumn] method.


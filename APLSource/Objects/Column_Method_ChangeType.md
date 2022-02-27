# ChangeType Method

## Applies To

[Column]()

## Purpose

This method converts the data type of a column.


## Syntax

~~~
  R=I.ChangeType X
~~~

X is the new datatype. R is instance I.


## Notes

For example:

~~~
      C.ChangeType 'Float'
~~~

Demoting a column is disallowed if the demotion will result in a loss of data or precision.  System
column types may not be changed.


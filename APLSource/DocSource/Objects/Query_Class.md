# Query Object

## Purpose

Represents a query.

## Description

The Query object provides full-featured query functionality,
including grouping and arranging of data, temporary columns or calculations, and
column renaming. In addition, past states of the database may be queried.

A Query object is obtained by executing the Query method of a Table object,
which defines the starting table for the query:

~~~
      D=S.OpenDatabase 'SandP'
      T=S.GetTable S
      Q=T.Query ""
~~~

The query is executed by running the [Execute]() method, which returns a 
DataTable object:

~~~
      R=Q.Execute 0
~~~

The Where and Select properties may optionally be set when the Query object is created by passing them as parameters to the Table.Query method:

~~~
      Q=T.Query 'CITY eq London' 'SNO,SNAME'
~~~~

Or they may be subsequently set after the Query object is created:

~~~
      Q.Where=''CITY eq London''  '
      Q.Select=''SNO,SNAME''  '
~~~

Some simple queries on the Suppliers and Parts database:

Select all rows and all columns of the Parts table:

~~~
      T=D.GetTable 'P'
      Q=T.Query ''
      R=Q.Execute 0
~~~


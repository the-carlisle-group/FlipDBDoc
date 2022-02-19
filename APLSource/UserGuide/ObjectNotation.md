# JSON Representation of FlipDB Objects

Every FlipDB object has a JSON (JavaScript Object Notation) representation suitable for
transfer between client and server over the Web.

## Representing DataColumns

DataColumns map directly to JSON numbers, strings, and arrays. A scalar character column in FlipDB:

~~~
      'Hello World'
┌───────────┐
│Hello World│
└Char(11)───┘
~~~

is a character string in JSON:

~~~
      "Hello World"
~~~

A scalar integer, decimal or float in FlipDB:

~~~
      6.125
┌──────┐
│6.125 │
└Dec(3)┘
~~~

Is a number in JSON:

~~~
     6.125
~~~

A date in FlipDB:

~~~
      '2017-12-31'
┌──────────┐
│2017-12-31│
└Date──────┘
~~~

is a string in JSON:

~~~
     "2017-12-31"
~~~

And a DateTime in FlipDB

~~~
       '2017-12-31 23:59:59'
┌───────────────────┐
│2017-12-31 23:59:59│
└DateTime───────────┘
~~~

is a string in JSON:
~~~
  "2017-12-31T23:59:59:000Z"
~~~

Simple DataColumns in FlipDB are represented by arrays in JSON. For example

~~~
      'Red,Green,Blue'
┌───────┐
↓Red    │
│Green  │
│Blue   │
└Char(5)┘
~~~

in JSON is written as:

~~~
      ["Red", Green", "Blue"]
~~~

Is is sometimes necessary to more formally represent FlipDB DataColumns in JSON.
This can be done using a JSON object:

~~~
   {
     "Class": "DataColumn",
     "Type" : "Date",
     "Values: ["2012-12-31", "1987-09-01", "2007-08-05"]
   }
~~~
This allows for typed empty arrays, which is not possible with the simple syntax.

## Representing DataTables
A DataTable is represented as a JSON object. There a few different acceptable formats.
The simplest format specifies the column names and the values as rows:

~~~
     { "Class": "DataTable",
       "ColumnNames": ["FirstName", "DOB", "Height"],
       "RowValues":[
                ["Bill", "1973-12-04", 75],
                ["Joe", "1964-07-31", 68],
                ["Bob", "1989-01-26", 71]
          ]
     }
~~~

In this case, the consuming application determines the column types by inspection.
The data in the table may alternatively be specified by column rather than by row:

~~~
    { "Class": "DataTable",
       "ColumnNames": ["FirstName", "DOB", "Height"],
       "ColumnValues":[
                ["Bill", "Joe", "Bob"],
                ["1973-12-04", "1964-07-31", "1989-01-26"],
                [75, 68, 71]
          ]
    }
~~~

A DataTable may also be represented by an array of DataColumns:

~~~
   {
      "Class": "DataTable",
      "Columns: [
                {"Class":"DataColumn",
                 "Name":"FirstName",
                 "Type":"Char(4)",
                 "DisplayName":"First Name",
                 "Values":["Bill", "Joe", "Bob"]
                 },

                {

                },


                ]

~~~

Or an array of row objects:
~~~
       {
       "Class":"DataTable",
        "Rows":[{"FirstName":"Bill"
                 "DOB": " "
                 "Height":73
                },




~~~




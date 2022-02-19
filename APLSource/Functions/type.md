# type

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

information{.purpose}

~~~
R=type X
~~~

X may be any column. R is a Char scalar representing the data type of X.

R is one of "Char", "Boolean", "Integer", "Decimal", "Float", "Date", "DateTime", "Time" or "Blob".

See also: →[structure]

## Examples

~~~
      type 'Hello'
┌───────┐
│Char   │
└Char(4)┘
      type 6
┌───────┐
│Integer│
└Char(7)┘
      type 3.14
┌───────┐
│Decimal│
└Char(7)┘
      type '2025-12-31'
┌───────┐
│Date   │
└Char(4)┘
~~~


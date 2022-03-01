# Type

DataColumns have type.  When a column is displayed in the session, its type is noted at the
bottom of the surrounding display box. FlipDB supports a character or string type Char:

~~~
      'Hello world!'
┌────────────┐
│Hello world!│
└Char(12)────┘
~~~

Unicode is supported as well with type CharW (where the W stands for "wide"), so we can write hello
world in Greek:

~~~
      'Γεια σας κόσμο'
┌──────────────┐
│Γεια σας κόσμο│
└CharW(14)─────┘
~~~

Char uses 1-byte per character, and CharW uses 2 bytes per character. In addition there is a CharXW
(XW for extra wide) type that uses 4 bytes per character. The value in parentheses for these types
is the length in characters.

FlipDB supports a boolean type. Boolean values are represented by zeros which are interpreted as "false":

~~~
      0
┌───────┐
│0      │
└Boolean┘
~~~

And by ones, which are interpreted as "true":

~~~
      1
┌───────┐
│1      │
└Boolean┘
~~~

Note that boolean types in FlipDB are in fact numeric, and may be treated as such. Thus we may add
up, say, three boolean values:

~~~
      1 + 0 + 1
┌────┐
│2   │
└Int8┘
~~~

This returns the value 2, which takes us to our next type, integer. Integer types come in three
sizes.  Int8, or 8 bit integer, handles numbers up to 127:

~~~
      127
┌────┐
│127 │
└Int8┘
~~~

Int16, or 16 bit integer, handles numbers up to 32,767:

~~~
      32767
┌──────┐
│32,767│
└Int16─┘
~~~

And finally, Int32, handles numbers up to well over 2 billion:

~~~
      2147483647
┌─────────────┐
│2,147,483,647│
└Int32────────┘
~~~

Larger numbers are handled by the float and decimal types. The decimal type includes the precision,
or number of decimal places:

~~~
      3.14
┌──────┐
│3.14  │
└Dec(2)┘
~~~

After 9 decimal places, when entering a literal, FlipDB uses the Float type:

~~~
      3.1415926535
┌────────────┐
│3.1415926535│
└Float───────┘
~~~

FlipDB supports types for dates and times. Literal dates are entered as appropriately formatted
character strings:

~~~
      '2012/05/10'
┌──────────┐
│2012-05-10│
└Date──────┘
~~~

Similarly for a time, which is always specified in a 24 hour format:

~~~
      '16:15:32'
┌────────┐
│16:15:32│
└Time────┘
~~~

And finally, a date and time may be represented with a DateTime type:

~~~
      '2012/05/10 16:15:32'
┌───────────────────┐
│2012-05-10 16:15:32│
└DateTime───────────┘
~~~

All of these types are broadly grouped into three basic categories: character, numeric, and date.
The distinction within each category generally only becomes important when designing a database
schema and considering storage and performance. When programming and applying functions and doing
ad hoc analysis, there is little to no need to worry about the details of the types. FlipDB
handles the details for you, promoting types where possible. For example, we may easily add up
boolean, integer, and decimal values:

~~~
      0 + 5 + 7.438
┌──────┐
│12.438│
└Dec(3)┘
~~~


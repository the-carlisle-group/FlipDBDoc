# toDate

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=[Y] [Z] toDate X
~~~

Converts X to type Date according to the optional format(s) Y and century window Z. FlipDB
correctly handles dates in the range from 1753-01-01 to 3999-12-31.

X is any column but Blob, boolean and Time. If X is numeric, Y is ignored and may be omitted while
X is expected to be in ccyymmdd or yymmdd format with Z defining the century. If X is character, Y
is one or more appropriate date formats for parsing X. Each format is applied in succession and
the first valid date that results is returned. Date formats may be fixed, variable, or "Serial".

For best performance, especially with large data sets, use a fixed format if you can, a variable
format if you must, and the absolute minimum number of formats necessary.

## Fixed formats

Fixed formats have fixed positions for date elements year, month and day. Any permutation
of '[yy]yy', 'mm[m]' and 'dd' will be accepted. The year and
month elements are required. The day element is optional. If the century element is not provided,
the century is determined by Z. If the day element is not provided, the day of each date will
default to the first of the month. Within the data the digits of the dates must coincide exactly
with the positions the  elements hold in the format. Fixed formats may or
may not have delimiters; that is, the date elements don't need to be contiguous.
All characters other than 'y','m','d' in the format are simply space holders.
For example:

~~~
      'mmddyyyy' toDate '12311999,01012001'
┌──────────┐
↓1999/12/31│
│2001/01/01│
└Date──────┘
     'dd jan yy' toDate '31-Dec-99,01-Jan-00'
┌──────────┐
↓1999/12/31│
│2000/01/01│
└Date──────┘
~~~

## Fixed Formats With Missing Leading Zeros

A common occurance is an undelimited date field with missing leading zeros. These are handled by providing a single 'y', 'm' or 'd'
at the beginning of the date format string:

~~~
      'd mmm yy' toDate '31-Dec-99,1-Jan-00'
┌──────────┐
↓1999/12/31│
│2000/01/01│
└Date──────┘

      'mddyy' toDate '123199,13101,10101'
┌──────────┐
↓1999/12/31│
│2001/01/31│
│2001/01/01│
└Date──────┘
~~~

## Variable formats

Variable formats must include 'y', 'm', and 'd' in any order, to specify the relative positions of the date
elements. There are six possible variable format strings. Variable formatted dates must be delimited;
there must be at least one non-alphanumeric character
between each date element.  For example:

~~~
      'dmy' toDate '1-1-1999,31-12-00,28-02-2018'
┌──────────┐
↓1999/01/01│
│2000/12/31│
│2018/02/28│
└Date──────┘

     'mdy' toDate 'January 31 2012,July 4 2020'
┌──────────┐
↓2012/01/31│
│2020/07/04│
└Date──────┘

~~~

## The Serial Format

The "Serial" format converts Microsoft Excel serial numbers to dates. See the →[fromSerial] and
→[toSerial] functions for a full discussion of Excel compatibility. If X is numeric and contains
serial numbers, the →[fromSerial] function should be used instead. For example:

~~~
      'Serial'toDate '35000,35001,37123,40000'
┌──────────┐
↓1995/10/28│
│1995/10/29│
│2001/08/20│
│2009/07/06│
└Date──────┘
~~~

## Multiple Formats in One Column

Sometimes a column will contain multiple date formats. The left argument of toDate accepts
multiple format strings. For performance, format strings should be provided in the order of the
most commonly occuring date format. For example:

~~~
      'mdy,yyyymmdd,Serial'  toDate '7/24/64,27123,19891231,35000,1/1/01'
┌──────────┐
↓2064/07/24│
│1974/04/04│
│1989/12/31│
│1995/10/28│
│2001/01/01│
└Date──────┘
~~~

## Default Left Argument

The left argument defaults to the three most common variable formats in the following order:
~~~
'mdy'
'dmy'
'ymd'
~~~
For example:

~~~
      toDate '1/1/2012,12/31/2018,28/2/1999'
┌──────────┐
↓2012/01/01│
│2018/12/31│
│1999/02/28│
└Date──────┘
~~~

## Century window

The century window parameter Z defines either a fixed or floating window. Two digit years are moved
to be within the century defined by the window. If Z is less than 100 it defines a floating window
that started Z years before the current year. Otherwise a fixed century window starting at year Z
is defined. The default is 50. For example:

~~~
      'mdy' toDate '7/24/64,1/1/01'
┌──────────┐
↓2064/07/24│
│2001/01/01│
└Date──────┘

      'mdy' 70 toDate '7/24/64,1/1/01'
┌──────────┐
↓1964/07/24│
│2001/01/01│
└Date──────┘

      'mdy' 1900 toDate '7/24/64,1/1/01'
┌──────────┐
↓1964/07/24│
│1901/01/01│
└Date──────┘

      'mdy' 2000 toDate '7/24/64,1/1/01'
┌──────────┐
↓2064/07/24│
│2001/01/01│
└Date──────┘
~~~


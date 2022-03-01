# toSerial

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

Converts Dates into absolute Integer.{.purpose}

~~~
R=[X] toSerial Y
~~~

Y is Date. R is Integer representing the number of days difference between Y and a specific date
origin. R is commonly referred to as a serial number. X is an optional Boolean scalar and
specifies the date origin. X should be omitted or 0 when converting dates to serial numbers that
need to be compatible with Microsoft Excel.

If X is 0 or omitted, the date origin is December 30th 1899. This is compatible with Microsoft
Excel for serial numbers 61 and greater, which corresponds to dates on or after March 1st 1900.
There is a one day discrepancy for dates in January and February of 1900 which correspond to
serial numbers less than 61, due to long-standing behavior in Excel. If Y is a null date, R is
zero. While Excel itself does not handle dates earlier than January 1st 1900, Y may in fact be
earlier than December 30th 1899, which will yield negative serial numbers.

If X is 1, the date origin is one day before the start of the Common Era on the false assumption
that the Gregorian Calendar has been in effect for the entire period.

Dates prior to the adoption of the Gregorian calendar in the country under consideration will give
incorrect results.

See also: →[fromSerial]

## Examples

~~~
       toSerial '1999-12-31,2000-01-01'
┌──────┐
↓36,525│
│36,526│
└Int32─┘
       toSerial '1899-12-29,1899-12-30,1899-12-31'
┌────┐
↓(1) │
│0   │
│1   │
└Int8┘
       toSerial '0000-00-00'
┌────┐
│0   │
└Int8┘
       1 toSerial '1999-12-31,2000-01-01'
┌───────┐
↓730,119│
│730,120│
└Int32──┘
       1 toSerial '1899-12-29,1899-12-30,1899-12-31'
┌───────┐
↓693,593│
│693,594│
│693,595│
└Int32──┘
~~~


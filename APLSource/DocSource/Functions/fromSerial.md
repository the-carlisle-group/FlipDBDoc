# fromSerial

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

Re-converts serialised dates.{.purpose}

~~~
R=[X] fromSerial Y
~~~

Y is Integer representing the number of days difference between a date. and some specific date
origin. R is Date and is the date that corresponds to Y. X is an optional Boolean scalar and
specifies the date origin. X should be 0 or omitted when converting serial numbers generated in
Microsoft Excel.

If X is 0 or omitted, the date origin is December 30th 1899. This is compatible with Microsoft
Excel for serial numbers 61 and greater, which corresponds to dates on or after March 1st 1900.
There is a one day discrepancy for dates in January and February of 1900 which corresponds to
serial numbers less than 61, due to a long-standing bug in Excel. If Y is zero, R is a null date.
While Excel itself does not handle dates earlier than January 1st 1900, Y may in fact be negative,
and R will then be a date earlier than the date origin of December 30th 1899.

If X is 1, then the date origin is one day before the start of the Common Era on the false
assumption that the Gregorian Calendar has been in effect for the entire period.

This function can be used to retrieve the actual date values of the results of a calculation on
integers previously converted from dates with the toSerial function. In this case, care should
be taken to ensure the same date origin, or value for X, is used in both conversions. In addition,
it should be noted that fromSerial and toSerial are NOT inverses of each other when the date in
question is the date origin itself. Therefore, computations that involve December 30th, 1899
should always use 1 as a value for X.

See also: →[toSerial]

## Examples

~~~
      fromSerial  -1 0 1 2 60 61 36525 36526
┌──────────┐
↓1899-12-29│
│          │
│1899-12-31│
│1900-01-01│
│1900-02-28│
│1900-03-01│
│1999-12-31│
│2000-01-01│
└Date──────┘
      1 fromSerial 0 1 2 730119 730120
┌──────────┐
↓          │
│   1-01-01│
│   1-01-02│
│1999-12-31│
│2000-01-01│
└Date──────┘
~~~


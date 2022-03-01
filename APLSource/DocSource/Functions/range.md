# range

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Statistical,Misc{.info}

~~~
R=X range Y
~~~

Y must be a simple Integer, Decimal, or Date column. (Float values must be explicitly converted to
Decimal using →[toDecimal] or →[round] before ranging.) X determines how Y is to be ranged. Acceptable
values for X depend on the type of Y. The result R is the same type as Y. Values in Y are
converted to the lower bound of the bucket in which they would appear.

Note that while the result of the range function is the same type as the argument, FlipDB maintains
internally the complete range specification so that reports may be appropriately formatted.

## Ranging Numeric Columns.

For columns of type Integer or Decimal, a range specification X is an increasing series of break
points. For example:

~~~
20 30 40 50
~~~

If the range specification is a triple, with the last item less than the middle item, then the
argument is interpreted as a “from-to-by” triple, rather than an explicit set of break points. For example:

~~~
20 80 10
~~~

is equivalent to:

~~~
20 30 40 50 60 70 80
~~~

(Note that each element in a from-to-by triple must be non-negative in order to be interpreted as such.)

If the range specification is a scalar, it is interpreted as a bucket size, with the number of
buckets determined by the minimum and the maximum of the data. For example, a range of 10 on data
with a minimum value of 22 and a maximum value of 55 is equivalent to:

~~~
20 30 40 50 60
~~~

The break points represent the lower, inclusive bound of the range. The upper bound is implied by
the following break point (thus N break points imply N-1 buckets). For example, for an integer
column, the break points 20,30,40,50 define the ranges 20 to 29, 30 to 39, and 40 to 49, while
the break points  1,21,31, 41 define the ranges 1 to 20, 21 to 30, and 31 to 40.   For a decimal
column of precision 3, the break points 5, 5.5, 6, 6.5 defines the range buckets 5.000 to 5.499,
5.500 to 5.999, and 6.000 to 6.499, while the break points 5.001, 5.501, 6.001, 6.501 defines the
range buckets 5.001 to 5.500, 5.501 to 6.000, 6.001 to 6.500.

A range definition cannot exclude data. If a range is specified such that it does not include all
of the data, then a top or bottom bucket (or both) will be added automatically. These additional
buckets will be exactly sized to fit the extra data, indicating the minimum and maximum values in
the column.

## Ranging Date Columns

Ranging date columns works in a similar manner to numeric columns. Break points may be specified as
an ascending series of dates, and a from-to-by triple may be specified as two dates and an
integer. If the integer is positive, it is interpreted as a number of days. If the integer is
negative, it is interpreted as a number of months. As with the numeric columns, the "from" and
"to" may be elided, and only an integer provided, indicating the bucket size in days or months.


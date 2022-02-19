# toDateTime

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=toDateTime X
~~~

X may be any column. R is DateTime.

If X is numeric, it represents a datetime as "CCYYMMDD.hhmmss". Trailing digits after the decimal
point may be omitted. A number with more or less than 8 integer digits will be scaled such that:
201003191230 will be treated as 20100319.123000 and 2010032 as 20100320.000000. Numbers
representing invalid dates or datetimes will result in a zero (null) date.

If X is Char, only digits are kept from the string, it is converted to a number and the above
process is followed.

If X is Blob, it is converted first to Char, then to DateTime.

## Examples

~~~
      v='20100102,2010-12-31,abc1799-01-02foo00:59:59'
      toDateTime v
┌───────────────────┐
↓2010-01-02 00:00:00│
│2010-12-31 00:00:00│
│1799-01-02 00:59:59│
└DateTime───────────┘
      v='Babble,20000000 090909,19990102 999999'
      toDateTime v
┌───────────────────┐
↓                   │
│                   │
│                   │
└DateTime───────────┘
      v=18001231.235959 19001231 0 20001231.1
      toDateTime v
┌───────────────────┐
↓1800-12-31 23:59:59│
│1900-12-31 00:00:00│
│                   │
│2000-12-31 10:00:00│
└DateTime───────────┘
~~~


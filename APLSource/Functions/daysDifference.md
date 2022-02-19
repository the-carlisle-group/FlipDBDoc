# daysDifference

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Days difference between two dates.{.purpose}

~~~
R=X daysDifference Y
~~~

X and Y must be Date or DateTime. R is Integer. R is the number of days between the starts of the
days in X and Y. R will be negative if Y is greater than X. If either X or Y is zero (null), R is zero.

See also: →[monthsDifference], →[yearsDifference]

## Examples

~~~
      d='2012-01-01,2011-07-24,0000-00-00'
      d
┌──────────┐
↓2012-01-01│
│2011-07-24│
│          │
└Date──────┘
      '2012-2-01'daysDifference d
┌─────┐
↓31   │
│192  │
│0    │
└Int16┘
      d2='2012-03-01,2011-07-20,2007-01-01'
      d2 d (d2 daysDifference d)
 ┌──────────┐  ┌──────────┐  ┌────┐
 ↓2012-03-01│  ↓2012-01-01│  ↓60  │
 │2011-07-20│  │2011-07-24│  │(4) │
 │2007-01-01│  │          │  │0   │
 └Date──────┘  └Date──────┘  └Int8┘
~~~


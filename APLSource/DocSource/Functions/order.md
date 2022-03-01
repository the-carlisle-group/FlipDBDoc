# order

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Misc{.info}

Compute the indices for sorting on one or more columns.{.purpose}

~~~
R=order Y
~~~

Y is one or more sort directives. R is a permutation of the indices back into the columns of Y.

order is often used with the →[*.by] operator.

Note that when the order function is used with only one sort directive, it is equivalent
to →[gradeUp] or →[gradeDown], which have a simpler syntax.

See also: →[*.by], →[*.uniqueValues], →[*.group], →[*.gradeUp], →[*.gradeDown], →[*.sortUp], →[*.sortDown]

## Examples

~~~
      d='2000/7/1,1987/12/31,2012/8/1,2001/6/15,2005/8/11'
      c='A,B,A,B,A'
      i=order c 'Asc'
      i c d
 ┌────┐  ┌───────┐  ┌──────────┐
 ↓0   │  ↓A      │  ↓2000-07-01│
 │2   │  │B      │  │1987-12-31│
 │4   │  │A      │  │2012-08-01│
 │1   │  │B      │  │2001-06-15│
 │3   │  │A      │  │2005-08-11│
 └Int8┘  └Char(1)┘  └Date──────┘
      i (i index each c d)
 ┌────┐   ┌───────┐  ┌──────────┐
 ↓0   │   ↓A      │  ↓2000-07-01│
 │2   │   │A      │  │2012-08-01│
 │4   │   │A      │  │2005-08-11│
 │1   │   │B      │  │1987-12-31│
 │3   │   │B      │  │2001-06-15│
 └Int8┘   └Char(1)┘  └Date──────┘
      j=order (c 'Asc') (d 'Desc')
      j c d
 ┌────┐  ┌───────┐  ┌──────────┐
 ↓2   │  ↓A      │  ↓2000-07-01│
 │4   │  │B      │  │1987-12-31│
 │0   │  │A      │  │2012-08-01│
 │3   │  │B      │  │2001-06-15│
 │1   │  │A      │  │2005-08-11│
 └Int8┘  └Char(1)┘  └Date──────┘
     j  (j index each c d)
 ┌────┐   ┌───────┐  ┌──────────┐
 ↓2   │   ↓A      │  ↓2012-08-01│
 │4   │   │A      │  │2005-08-11│
 │0   │   │A      │  │2000-07-01│
 │3   │   │B      │  │2001-06-15│
 │1   │   │B      │  │1987-12-31│
 └Int8┘   └Char(1)┘  └Date──────┘
~~~

When order is used with only one sort directive, it is equivalent to gradeUp or gradeDown:

~~~
      i=gradeDown d
      j=gradeUp i index c
      (j index i) index each c d
 ┌───────┐  ┌──────────┐
 ↓A      │  ↓2012-08-01│
 │A      │  │2005-08-11│
 │A      │  │2000-07-01│
 │B      │  │2001-06-15│
 │B      │  │1987-12-31│
 └Char(1)┘  └Date──────┘
~~~


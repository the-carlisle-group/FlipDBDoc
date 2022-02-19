# join

Type:{.prefix}

Table{.info}

Categories:{.prefix}

Relational{.info}

~~~
R=X join Y
~~~

X and Y and R are DataTables. R is the relational inner join of X and Y on the columns defined by
the Key properties of X and Y. If no Key is specified in X or Y, R is a cross join of X and Y. R
is composed of all columns in X with those columns in Y that do not exist in X.

## Examples:

An inner join compares each row in the first table with each row in the second table. If the key
values of the two rows are equal, the rows are combined, and this combined row is included in the
result.  Thus rows with no matching key are discarded in the result:

~~~
      t1=('Col1' 'A,B,C,D')('Col2' (1 2 3 4))
      t1.Key='Col1'
      t2=('Col3' 'E,B,A') ('Col4' (5 6 7))
      t2.Key='Col3'
      t1 t2
 ── Key:Col1 ───────────  ── Key:Col3 ───────────
  ┌Col1───┐  ┌Col2┐        ┌Col3───┐  ┌Col4┐
  ↓A      │  ↓1   │        ↓E      │  ↓5   │
  │B      │  │2   │        │B      │  │6   │
  │C      │  │3   │        │A      │  │7   │
  │D      │  │4   │        └Char(1)┘  └Int8┘
  └Char(1)┘  └Int8┘       ── 3 rows by 2 columns
 ── 4 rows by 2 columns
      t1 join t2
──────────────────────────────────────
 ┌Col1───┐  ┌Col2┐  ┌Col3───┐  ┌Col4┐
 ↓A      │  ↓1   │  ↓A      │  ↓7   │
 │B      │  │2   │  │B      │  │6   │
 └Char(1)┘  └Int8┘  └Char(1)┘  └Int8┘
── 2 rows by 4 columns ───────────────
~~~

While duplicate key values in the second table create additional rows in the result:

~~~
      t1=('Col1' 'A,A,B,C')('Col2' (1 2 3 4))
      t1.Key='Col1'
      t2=('Col3' 'A,X,A') ('Col4' (5 6 7))
      t2.Key='Col3'
      t1 t2
 ── Key:Col1 ───────────  ── Key:Col3 ───────────
  ┌Col1───┐  ┌Col2┐        ┌Col3───┐  ┌Col4┐
  ↓A      │  ↓1   │        ↓A      │  ↓5   │
  │A      │  │2   │        │X      │  │6   │
  │B      │  │3   │        │A      │  │7   │
  │C      │  │4   │        └Char(1)┘  └Int8┘
  └Char(1)┘  └Int8┘       ── 3 rows by 2 columns
 ── 4 rows by 2 columns
      t1 join t2
──────────────────────────────────────
 ┌Col1───┐  ┌Col2┐  ┌Col3───┐  ┌Col4┐
 ↓A      │  ↓1   │  ↓A      │  ↓5   │
 │A      │  │1   │  │A      │  │7   │
 │A      │  │2   │  │A      │  │5   │
 │A      │  │2   │  │A      │  │7   │
 └Char(1)┘  └Int8┘  └Char(1)┘  └Int8┘
── 4 rows by 4 columns ───────────────
      t2 join t1
──────────────────────────────────────
 ┌Col3───┐  ┌Col4┐  ┌Col1───┐  ┌Col2┐
 ↓A      │  ↓5   │  ↓A      │  ↓1   │
 │A      │  │5   │  │A      │  │2   │
 │A      │  │7   │  │A      │  │1   │
 │A      │  │7   │  │A      │  │2   │
 └Char(1)┘  └Int8┘  └Char(1)┘  └Int8┘
── 4 rows by 4 columns ───────────────
~~~

If no key is specified (or if the key values are all identical in and across both files) then a
cross join is performed. A cross join contains all combinations of rows from both tables:

~~~
      t1.Key=''
      t2.Key=''
      t3=t1 join t2
      t3
──────────────────────────────────────
 ┌Col1───┐  ┌Col2┐  ┌Col3───┐  ┌Col4┐
 ↓A      │  ↓1   │  ↓A      │  ↓5   │
 │A      │  │1   │  │X      │  │6   │
 │A      │  │1   │  │A      │  │7   │
 │A      │  │2   │  │A      │  │5   │
 │A      │  │2   │  │X      │  │6   │
 │A      │  │2   │  │A      │  │7   │
 │B      │  │3   │  │A      │  │5   │
 │B      │  │3   │  │X      │  │6   │
 │B      │  │3   │  │A      │  │7   │
 │C      │  │4   │  │A      │  │5   │
 │C      │  │4   │  │X      │  │6   │
 │C      │  │4   │  │A      │  │7   │
 └Char(1)┘  └Int8┘  └Char(1)┘  └Int8┘
── 12 rows by 4 columns ──────────────
~~~

An inner join may be constructed as where clause applied to a cross join, selecting those rows with
equal key values:

~~~
      t3.ExecuteQuery 'Col1 eq Col3' ''
──────────────────────────────────────
 ┌Col1───┐  ┌Col2┐  ┌Col3───┐  ┌Col4┐
 ↓A      │  ↓1   │  ↓A      │  ↓5   │
 │A      │  │1   │  │A      │  │7   │
 │A      │  │2   │  │A      │  │5   │
 │A      │  │2   │  │A      │  │7   │
 └Char(1)┘  └Int8┘  └Char(1)┘  └Int8┘
── 4 rows by 4 columns ───────────────
~~~


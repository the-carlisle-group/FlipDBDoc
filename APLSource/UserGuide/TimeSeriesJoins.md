# Timeseries Joins

A time series table is any table that contains a Date or DateTime column as part of its primary
key. Let's open up the extended suppliers and parts database, and review the Part table:

~~~
      d=Scripting.Server.OpenDatabase 'SandPX'
      d.GetTable 'Part'
── SandPX.Part ────────────────────────────────────────
 ┌PartID─┐  ┌Name───┐  ┌Color──┐  ┌Weight┐  ┌City────┐
 ↓P1     │  ↓Nut    │  ↓Red    │  ↓12    │  ↓London  │
 │P2     │  │Bolt   │  │Green  │  │17    │  │Paris   │
 │P3     │  │Screw  │  │Blue   │  │17    │  │Oslo    │
 │P4     │  │Screw  │  │Red    │  │14    │  │London  │
 │P5     │  │Cam    │  │Blue   │  │12    │  │Paris   │
 │P6     │  │Cog    │  │Red    │  │19    │  │London  │
 └Char(2)┘  └Char(5)┘  └Char(5)┘  └Int8──┘  └Char(10)┘
── 6 rows by 5 columns ────────────────────────────────
~~~

And now the Price table, which contains the prices over time for each part, and is thus a time
series table:

~~~
      d.GetTable 'Price'
── SandPX.Price ────────────────────────────────────────────
 ┌AUTOKEY┐  ┌PartID─┐  ┌AsOfDate──┐  ┌Price─┐  ┌DiscountID┐
 ↓1      │  ↓P1     │  ↓2012-01-15│  ↓1.15  │  ↓A         │
 │2      │  │P2     │  │2012-01-15│  │3.25  │  │A         │
 │3      │  │P3     │  │2012-01-15│  │2.75  │  │A         │
 │4      │  │P1     │  │2012-01-16│  │1.18  │  │B         │
 │5      │  │P2     │  │2012-01-16│  │3.15  │  │B         │
 │6      │  │P3     │  │2012-01-16│  │2.80  │  │A         │
 │7      │  │P1     │  │2012-01-17│  │1.25  │  │A         │
 │8      │  │P2     │  │2012-01-17│  │3.05  │  │B         │
 │9      │  │P3     │  │2012-01-17│  │2.75  │  │C         │
 │10     │  │P1     │  │2012-01-18│  │1.35  │  │B         │
 │11     │  │P2     │  │2012-01-18│  │2.95  │  │C         │
 │12     │  │P3     │  │2012-01-18│  │2.80  │  │C         │
 │13     │  │P1     │  │2012-01-19│  │1.40  │  │B         │
 │14     │  │P3     │  │2012-01-20│  │2.70  │  │A         │
 └Int8───┘  └Char(2)┘  └Date──────┘  └Dec(2)┘  └Char(1)───┘
── 14 rows by 5 columns ────────────────────────────────────
~~~

In the Price table, the combination of PartID and AsOfDate is unique, and PartID is a foreign key
pointing back to the Part table.

A time series join allows us to bring columns from the Price table into the Part table. Let's
create a query on the Part table, with a few columns:

~~~
      q=(d.GetTable 'Part').Query '' 'PartID,Name'
~~~

And then, using time series join syntax, bring in the Price column from the Price table for two
different time periods:

~~~
      j=q.AddColumn 'PriceJan18' 'Price[2012/01/18].Price'
      j=q.AddColumn 'PriceJan17' 'Price[2012/01/17].Price'
~~~

Now let's execute the query:

~~~
      q.Execute 0
── Key:PartID ────────────────────────────────────
 ┌PartID─┐  ┌Name───┐  ┌PriceJan18┐  ┌PriceJan17┐
 ↓P1     │  ↓Nut    │  ↓1.35      │  ↓1.25      │
 │P2     │  │Bolt   │  │2.95      │  │3.05      │
 │P3     │  │Screw  │  │2.80      │  │2.75      │
 │P4     │  │Screw  │  │0.00      │  │0.00      │
 │P5     │  │Cam    │  │0.00      │  │0.00      │
 │P6     │  │Cog    │  │0.00      │  │0.00      │
 └Char(2)┘  └Char(5)┘  └Dec(2)────┘  └Dec(2)────┘
── 6 rows by 4 columns ───────────────────────────
~~~

And review the Select property:

~~~
      q.Select
────────────────────────────────────────────────────
 ┌Name──────┐  ┌Expression─────────────┐  ┌Visible┐
 ↓PartID    │  ↓PartID                 │  ↓1      │
 │Name      │  │Name                   │  │1      │
 │PriceJan18│  │Price[2012/01/18].Price│  │1      │
 │PriceJan17│  │Price[2012/01/17].Price│  │1      │
 └Char(10)──┘  └Char(23)───────────────┘  └Boolean┘
── 4 rows by 3 columns ─────────────────────────────
~~~

The time series syntax allows us to bring in the values for price for a specific date.  In this
query result, we can see that the price for a bolt on January 18th was 2.95, down from 3.05 on
January 17th.

Note that all of the prices for P4, P5 and P6 are zero. This is because there are no entries for
those part IDs on those dates in the Price table, and zero is the fill value for numeric columns.
We could attempt to identify those rows in Parts for which we have no data in the Price table by
noting where, say, PriceJan18 is zero. However, zero may in fact be a valid value. A better way is
to directly inspect the AsOfDate in the Price table. Let's add the AsOfDate to the query and re-execute:

~~~
      j=q.AddColumn 'AsOfDateJan18' 'Price[2012/01/18].AsOfDate'
      q.Execute 0
── Key:PartID ─────────────────────────────────────────────────────
 ┌PartID─┐  ┌Name───┐  ┌PriceJan18┐  ┌PriceJan17┐  ┌AsOfDateJan18┐
 ↓P1     │  ↓Nut    │  ↓1.35      │  ↓1.25      │  ↓2012-01-18   │
 │P2     │  │Bolt   │  │2.95      │  │3.05      │  │2012-01-18   │
 │P3     │  │Screw  │  │2.80      │  │2.75      │  │2012-01-18   │
 │P4     │  │Screw  │  │0.00      │  │0.00      │  │             │
 │P5     │  │Cam    │  │0.00      │  │0.00      │  │             │
 │P6     │  │Cog    │  │0.00      │  │0.00      │  │             │
 └Char(2)┘  └Char(5)┘  └Dec(2)────┘  └Dec(2)────┘  └Date─────────┘
── 6 rows by 5 columns ────────────────────────────────────────────
~~~

Now we can explicitly check where AsOfDateJan18 is in fact equal to "2012/01/18" to determine which
rows in the Parts table have corresponding entries in the Price table for January 18th.  This
would most likely be done in the Where clause of the query:

~~~
      q.Where = '"2012/01/18" eq Price[2012/01/18].AsOfDate'
      q.Execute 0
── Key:PartID ─────────────────────────────────────────────────────
 ┌PartID─┐  ┌Name───┐  ┌PriceJan18┐  ┌PriceJan17┐  ┌AsOfDateJan18┐
 ↓P1     │  ↓Nut    │  ↓1.35      │  ↓1.25      │  ↓2012-01-18   │
 │P2     │  │Bolt   │  │2.95      │  │3.05      │  │2012-01-18   │
 │P3     │  │Screw  │  │2.80      │  │2.75      │  │2012-01-18   │
 └Char(2)┘  └Char(5)┘  └Dec(2)────┘  └Dec(2)────┘  └Date─────────┘
── 3 rows by 5 columns ────────────────────────────────────────────
~~~

Note that we could also check where the AsOfDateJan18 column is not equal to 0, which would be a
cleaner solution. This works for date columns, where it did not work for numeric columns, as zeros
are invalid dates.

In the examples so far we have specified that we want a column value from the time series table for
a specific date and if no entry is found, then a zero or blank value is returned. However, it is
sometimes the case that we want the nearest observation on or before a specific date, or on or
after a specific date. Let's create a new query on the parts table:

~~~
      q=(d.GetTable 'Part').Query ''
      j=q.AddColumn 'Part' 'PartID'
~~~

And add a column for the price of the part on or before January 19th:

~~~
      j=q.AddColumn 'Price1' 'Price[<=2012/1/19].Price'
~~~

...and a column for the price on exactly January 19th:

~~~
      j=q.AddColumn 'Price2' 'Price[==2012/1/19].Price'
~~~

... and a column for the price on or after January 19th:

~~~
      j=q.AddColumn 'Price3' 'Price[>=2012/1/19].Price'
~~~

And now inspect the results:

~~~
      q.Execute 0
─────────────────────────────────────────
 ┌Part───┐  ┌Price1┐  ┌Price2┐  ┌Price3┐
 ↓P1     │  ↓1.40  │  ↓1.40  │  ↓1.40  │
 │P2     │  │2.95  │  │0.00  │  │0.00  │
 │P3     │  │2.80  │  │0.00  │  │2.70  │
 │P4     │  │0.00  │  │0.00  │  │0.00  │
 │P5     │  │0.00  │  │0.00  │  │0.00  │
 │P6     │  │0.00  │  │0.00  │  │0.00  │
 └Char(2)┘  └Dec(2)┘  └Dec(2)┘  └Dec(2)┘
── 6 rows by 4 columns ──────────────────
~~~

It is sometime the case that we are interested in the most recent time series observation, whatever
date that may be.  In times series join syntax, there are four key words  that we may use in place
of an explicit date. Let's create a new query from the part table and explore each one:

~~~
      q=(d.GetTable 'Part').Query '' 'PartID,Name'
      j=q.AddColumn  'MaxDatePrice' 'Price[max].Price'
      j=q.AddColumn  'MinDatePrice' 'Price[min].Price'
      j=q.AddColumn  'LastDatePrice' 'Price[last].Price'
      j=q.AddColumn  'FirstDatePrice' 'Price[first].Price'
      q.Execute 0
── Key:PartID ───────────────────────────────────────────────────────────────────────────
 ┌PartID─┐  ┌Name───┐  ┌MaxDatePrice┐  ┌MinDatePrice┐  ┌LastDatePrice┐  ┌FirstDatePrice┐
 ↓P1     │  ↓Nut    │  ↓0.00        │  ↓1.15        │  ↓1.40         │  ↓1.15          │
 │P2     │  │Bolt   │  │0.00        │  │3.25        │  │2.95         │  │3.25          │
 │P3     │  │Screw  │  │2.70        │  │2.75        │  │2.70         │  │2.75          │
 │P4     │  │Screw  │  │0.00        │  │0.00        │  │0.00         │  │0.00          │
 │P5     │  │Cam    │  │0.00        │  │0.00        │  │0.00         │  │0.00          │
 │P6     │  │Cog    │  │0.00        │  │0.00        │  │0.00         │  │0.00          │
 └Char(2)┘  └Char(5)┘  └Dec(2)──────┘  └Dec(2)──────┘  └Dec(2)───────┘  └Dec(2)────────┘
── 6 rows by 6 columns ──────────────────────────────────────────────────────────────────
~~~

The "max" key word specifies that the maximum AsOfDate in the time series table should be
determined, and that this date then be used in the join. By inspection of the Price table we see
that the maximum date is January 20 but that only part P3 has an entry for this date, which is
for 2.70. Thus all the other values in MaxDatePrice are zero.

The "min" key word specifies that the minimum AsOfDate in the time series table should be
determined, and that this date should then be used in the join. Again by inspection, we see that
January 15th is the earliest date, and that parts P1, P2, and P3 have entries for this date.

The "last" key word specifies that the most recent AsOfDate for each key value should be used. That
implies that the results may not correspond to the same AsOfDate. In this case, the most recent
price for P3 is 2.70, as of January 20th, but the most recent price for P2 is 2.95, as of January
18th. Note that if all parts have entries for the maximum date then "last" and "max" will produce
the same result.

Finally, the "first" key word specifies the earliest AsOfDate for each key value should be used.
Like "last", this implies that the results may not correspond to the AsOfDate. However, in this
specific case they do, as the earliest date January 15th, and we have entries for P1, P2, and P3
for this date. Note that if all parts have entries for the maximum date then "first" and "min"
will produce the same result.

In addition to asking for the last or first observation using the "last" and "first" keywords,
we can ask for the next to the last, or, say, three after the first, or any particular offset. To
go back in time from the most recent observation, we negative integers:

~~~
      q=(d.GetTable 'Part').Query ''
      j=q.AddColumn 'Part' 'PartID'
      j=q.AddColumn 'Latest' 'Price[0].Price'
      j=q.AddColumn 'Previous' 'Price[-1].Price'
      j=q.AddColumn 'Back2' 'Price[-2].Price'
      j=q.AddColumn 'Back3' 'Price[-3].Price'
      q.Execute 0
─────────────────────────────────────────────────────
 ┌Part───┐  ┌Latest┐  ┌Previous┐  ┌Back2─┐  ┌Back3─┐
 ↓P1     │  ↓1.40  │  ↓1.35    │  ↓1.25  │  ↓1.18  │
 │P2     │  │2.95  │  │3.05    │  │3.15  │  │3.25  │
 │P3     │  │2.70  │  │2.80    │  │2.75  │  │2.80  │
 │P4     │  │0.00  │  │0.00    │  │0.00  │  │0.00  │
 │P5     │  │0.00  │  │0.00    │  │0.00  │  │0.00  │
 │P6     │  │0.00  │  │0.00    │  │0.00  │  │0.00  │
 └Char(2)┘  └Dec(2)┘  └Dec(2)──┘  └Dec(2)┘  └Dec(2)┘
── 6 rows by 5 columns ──────────────────────────────
~~~

Note that 0 represents the most recent observation and is thus equivalent to the keyword "last".
To go forward in time from the earliest observation we use positive integers:

~~~
      q=(d.GetTable 'Part').Query ''
      j=q.AddColumn 'Part' 'PartID'
      j=q.AddColumn 'Earliest' 'Price[first].Price'
      j=q.AddColumn 'Next' 'Price[1].Price'
      j=q.AddColumn 'Forward2' 'Price[2].Price'
      j=q.AddColumn 'Forward3' 'Price[3].Price'
      q.Execute 0
─────────────────────────────────────────────────────────
 ┌Part───┐  ┌Earliest┐  ┌Next──┐  ┌Forward2┐  ┌Forward3┐
 ↓P1     │  ↓1.15    │  ↓1.18  │  ↓1.25    │  ↓1.35    │
 │P2     │  │3.25    │  │3.15  │  │3.05    │  │2.95    │
 │P3     │  │2.75    │  │2.80  │  │2.75    │  │2.80    │
 │P4     │  │0.00    │  │0.00  │  │0.00    │  │0.00    │
 │P5     │  │0.00    │  │0.00  │  │0.00    │  │0.00    │
 │P6     │  │0.00    │  │0.00  │  │0.00    │  │0.00    │
 └Char(2)┘  └Dec(2)──┘  └Dec(2)┘  └Dec(2)──┘  └Dec(2)──┘
── 6 rows by 5 columns ──────────────────────────────────
~~~

Note that like the "last" and "first" keyword, using an integer implies that the results for a
particular column will not necessarily correspond to the same AsOfDate. In these cases the results
are determined simply by the ordering of the AsOfDates, not by their actual values.


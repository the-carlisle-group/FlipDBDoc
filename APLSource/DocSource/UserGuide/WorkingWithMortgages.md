# Mortgages

FlipDB is custom designed to deal with the complex importing, analysis, and reporting problems of
mortgage data. The →[*.MethodList.BuildSampleLoan] method creates a sample residential loan database:

~~~
      J=Scripting.Server.DeleteDatabase 'SampleLoan'
      D=Scripting.Server.BuildSampleLoan ''
      D.TableNames
┌──────────────┐
↓PropertyType  │
│OccupancyType │
│RateType      │
│LoanPurpose   │
│DocType       │
│Loan          │
│PaymentHistory│
└Char(14)──────┘
~~~

The Loan table is the main table, containing about 40 columns. This table is populated with 100
sample loans. The PaymentHistory table contains 36 months of payment history for these loans. The
remaining tables are small lookup tables for columns in the loan table.

## Summary Reporting on the Loan Table

Much mortgage reporting involves repeated queries on the Loan table on the same set of rows and the
same set of summary statistics, that is the same →[*.Where] and →[*.Select] properties. The
(mostly) same query is executed many times, with only the →[*.GroupBy] property changing each
time. To the extent that the Where property does not change, the totals row at the bottom of each
breakout may be computed once at the beginning. The process is as follows. First extract the loan table:

~~~
      T=D.GetTable 'Loan'
~~~

Then define the aggregate statistics, the Select property, by building a →[*.Objects.DataTable],
which will be used for each query:

~~~
      CS=('Name' emptyChar) ('Expression' emptyChar)
      J=CS.AddRows 'LoanCount' 'count LoanNumber'
      J=CS.AddRows 'TotalBalance'  'sum Balance'
      J=CS.AddRows 'Percent' '1 round 100 * TotalBalance / sum TotalBalance'
      J=CS.AddRows 'WAC' '3 round weightedAverage Rate Balance'
~~~

We have assigned this DataTable to the arbitrary name CS:

~~~
      CS
─────────────────────────────────────────────────────────────────
 ┌Name────────┐  ┌Expression───────────────────────────────────┐
 ↓LoanCount   │  ↓count LoanNumber                             │
 │TotalBalance│  │sum Balance                                  │
 │Percent     │  │1 round 100 * TotalBalance / sum TotalBalance│
 │WAC         │  │3 round weightedAverage Rate Balance         │
 └Char(12)────┘  └Char(45)─────────────────────────────────────┘
── 4 rows by 2 columns ──────────────────────────────────────────
~~~

Now create a query object, set the Select property and execute it to produce the total row:

~~~
      Q=T.Query ''
      Q.Select=CS
      Total=Q.Execute 0
      Total
──────────────────────────────────────────────────
 ┌LoanCount┐  ┌TotalBalance┐  ┌Percent┐  ┌WAC───┐
 │100      │  │7151277.07  │  │100    │  │7.346 │
 └Int8─────┘  └Dec(2)──────┘  └Dec(1)─┘  └Dec(3)┘
── 0 rows by 4 columns ───────────────────────────
~~~

For the first query, consider a breakout or grouping by mortgage coupon, in this case ranged from 5
to 10 in 50 basis point increments:

~~~
      Q=T.Query ''
      Q.Select=CS
      Q.GroupBy='Coupon' 'Rate' '5 10 .5'
      Scripting.ReportTable.New Q.Execute 0
                 Loan         Total
 Coupon         Count       Balance  Percent    WAC
────────────────────────────────────────────────────
 5.000 - 5.499      0          0.00      0.0  0.000
 5.500 - 5.999      0          0.00      0.0  0.000
 6.000 - 6.499      4    172,128.79      2.4  6.097
 6.500 - 6.999     19  1,556,259.11     21.8  6.614
 7.000 - 7.499     43  3,340,860.65     46.7  7.197
 7.500 - 7.999     12    649,901.96      9.1  7.780
 8.000 - 8.499      9    636,488.62      8.9  8.191
 8.500 - 8.999     13    795,637.94     11.1  8.643
 9.000 - 9.499      0          0.00      0.0  0.000
 9.500 - 9.999      0          0.00      0.0  0.000
──────a──────────────────────────────────────────────
~~~

Note that a new query object is created. While the same query object may be used repeatedly, it is
often easier to create a new one for each breakout, which ensures that all properties have their
default values. Note further that the DataTable that is the result of the query is passed to the
ReportTable.New method to produce a ReportTable. The display of the DataTable itself does not show
the range information:

~~~
      Q.Execute 0
────────────────────────────────────────────────────────────
 ┌Coupon┐  ┌LoanCount┐  ┌TotalBalance┐  ┌Percent┐  ┌WAC───┐
 ↓5     │  ↓ 0       │  ↓      0     │  ↓ 0     │  ↓0     │
 │5.5   │  │ 0       │  │      0     │  │ 0     │  │0     │
 │6     │  │ 4       │  │ 172128.79  │  │ 2.4   │  │6.097 │
 │6.5   │  │19       │  │1556259.11  │  │21.8   │  │6.614 │
 │7     │  │43       │  │3340860.65  │  │46.7   │  │7.197 │
 │7.5   │  │12       │  │ 649901.96  │  │ 9.1   │  │7.78  │
 │8     │  │ 9       │  │ 636488.62  │  │ 8.9   │  │8.191 │
 │8.5   │  │13       │  │ 795637.94  │  │11.1   │  │8.643 │
 │9     │  │ 0       │  │      0     │  │ 0     │  │0     │
 │9.5   │  │ 0       │  │      0     │  │ 0     │  │0     │
 └Dec(3)┘  └Int8─────┘  └Dec(2)──────┘  └Dec(1)─┘  └Dec(3)┘
── 10 rows by 5 columns ────────────────────────────────────
~~~

The Coupon column only shows the lowest break point of each range. This is done so that the
resulting data type of a ranged column is the same as the data type of the unranged column it is
based on. The DataTable knows that it is the result of ranged query, and maintains the range
information used by the ReportTable object.

The previously computed totals may now be appended to the result of the query, before it is
converted to a ReportTable. First we name the result of query, then tack on the totals row, and
finally display in the session as a ReportTable, just to make it easier to read:

~~~
      CouponBreakout=Q.Execute 0
      J=CouponBreakout.AddRows Total
      Scripting.ReportTable.New CouponBreakout
                 Loan         Total
 Coupon         Count       Balance  Percent    WAC
────────────────────────────────────────────────────
 5.000 - 5.499      0          0.00      0.0  0.000
 5.500 - 5.999      0          0.00      0.0  0.000
 6.000 - 6.499      4    172,128.79      2.4  6.097
 6.500 - 6.999     19  1,556,259.11     21.8  6.614
 7.000 - 7.499     43  3,340,860.65     46.7  7.197
 7.500 - 7.999     12    649,901.96      9.1  7.780
 8.000 - 8.499      9    636,488.62      8.9  8.191
 8.500 - 8.999     13    795,637.94     11.1  8.643
 9.000 - 9.499      0          0.00      0.0  0.000
 9.500 - 9.999      0          0.00      0.0  0.000
────────────────────────────────────────────────────
 Total:           100  7,151,277.07    100.0  7.346
~~~

Now create a breakout on the current balance:

~~~
      Q=T.Query ''
      Q.Select=CS
      Q.GroupBy='MortgageBalance' 'Balance' '0 200000 25000'
      BalanceBreakout= (Q.Execute 0).AddRows Total
      Scripting.ReportTable.New BalanceBreakout
 Mortgage                  Loan         Total
 Balance                  Count       Balance  Percent    WAC
──────────────────────────────────────────────────────────────
 0.00 - 24,999.99             5     84,364.51      1.2  7.030
 25,000.00 - 49,999.99       35  1,392,051.78     19.5  7.313
 50,000.00 - 74,999.99       26  1,613,152.26     22.6  7.610
 75,000.00 - 99,999.99       20  1,663,097.43     23.3  7.371
 100,000.00 - 124,999.99      4    447,001.84      6.3  7.771
 125,000.00 - 149,999.99      2    273,411.68      3.8  7.875
 150,000.00 - 174,999.99      4    625,360.17      8.7  7.027
 175,000.00 - 199,999.99      2    370,193.61      5.2  6.983
 200,000.00 - 394,755.39      2    682,643.79      9.5  6.764
 0.00 - 24,999.99           100  7,151,277.07    100.0  7.346
──────────────────────────────────────────────────────────────
~~~

Now for the origination date, grouped by year:

~~~
      Q=T.Query ''
      Q.Select=CS
      Q.GroupBy='OriginationYear' 'OriginationDate' '-12'
      OrigYearBreakout=(Q.Execute 0).AddRows Total
      Scripting.ReportTable.New OrigYearBreakout
 Origination   Loan         Total
 Year         Count       Balance  Percent    WAC
──────────────────────────────────────────────────
 2000            22  1,311,775.06     18.3  7.201
 2001            38  3,149,549.65     44.0  7.367
 2002            40  2,689,952.36     37.6  7.392
──────────────────────────────────────────────────
 Total:         100  7,151,277.07    100.0  7.346
~~~

And for the State:

~~~
      Q=T.Query ''
      Q.Select=CS
      Q.GroupBy='PropertyState' 'State'
      StateBreakout= (Q.Execute 0).AddRows Total
      Scripting.ReportTable.New StateBreakout
 Property   Loan         Total
 State     Count       Balance  Percent    WAC
───────────────────────────────────────────────
 VA           72  4,848,834.14     67.8  7.313
 GA            3    119,768.10      1.7  6.875
 NJ            6    731,853.79     10.2  7.133
 FL           10    642,600.62      9.0  7.342
 SC            2    106,022.06      1.5  8.529
 NC            3    207,858.80      2.9  8.179
 MD            4    494,339.56      6.9  7.495
───────────────────────────────────────────────
 Total:      100  7,151,277.07    100.0  7.346
~~~

For the Zip code, the result is ordered by total balance, and only the 5 largest zip codes
represented are shown:

~~~
      Q=T.Query ''
      Q.Select=CS
      Q.GroupBy='PropertyZip' 'Zip'
      Q.OrderBy='TotalBalance' 'Down'
      Q.HavingQuota=5
      Q.RollUp=1
      ZipBreakout= (Q.Execute 0).AddRows Total
      Scripting.ReportTable.New ZipBreakout
 Property   Loan         Total
 Zip       Count       Balance  Percent    WAC
───────────────────────────────────────────────
 23451         3    598,663.71      8.4  7.158
 23113         5    457,405.30      6.4  7.004
 07092         1    394,755.39      5.5  6.500
 23832         3    311,775.30      4.4  6.899
 22901         5    259,581.81      3.6  7.896
───────────────────────────────────────────────
 Total:      100  7,151,277.07    100.0  7.346
~~~

And finally for the occupancy status column:

~~~
      Q=T.Query ''
      Q.Select=CS
      Q.GroupBy='OccupancyStatus' 'OccupancyType'
      Q.KeepEmptyGroups=0
      OccupancyBreakout= (Q.Execute 0).AddRows Total
      Scripting.ReportTable.New OccupancyBreakout
 Occupancy   Loan         Total
 Status     Count       Balance  Percent    WAC
────────────────────────────────────────────────
 Primary       80  6,054,846.68     84.7  7.334
 Second         1     44,660.31      0.6  6.375
 Investor      19  1,051,770.08     14.7  7.454
────────────────────────────────────────────────
 Total:       100  7,151,277.07    100.0  7.346
~~~

Each of these query results may be added to a report:

~~~
      R=Scripting.Report.New ''
      J=R.Add 'Heading2' 'Coupon'
      J=R.Add CouponBreakout
      J=R.Add 'Heading2' 'Balance'
      J=R.Add BalanceBreakout
      J=R.Add 'Heading2' 'Year or Origination'
      J=R.Add OrigYearBreakout
      J=R.Add 'Heading2' 'State Distribution'
      J=R.Add StateBreakout
      J=R.Add 'Heading2' 'Top 5 Zip Codes'
      J=R.Add ZipBreakout
      R.Show ''
~~~


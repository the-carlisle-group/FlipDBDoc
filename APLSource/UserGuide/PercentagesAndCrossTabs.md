# Groups, Percentages, and Cross Tabs

Grouped queries often require various percentages in their results. To explore the use of
percentages, let's work with the Loan table of the built-in sample loan database. First, access
the server:

~~~
      S=Scripting.Server
~~~

Then, to make sure we have fresh copy, delete the existing sample loan database:

~~~
      J=S.DeleteDatabase 'SampleLoan'
~~~

...and then recreate it with a Loan table of 1,250 rows:

~~~
      D=S.BuildSampleLoan 'SampleLoan' 1250
~~~

And then extract the Loan table, and create a query:

~~~
      T=D.GetTable 'Loan'
      Q=T.Query ''
~~~

Now consider grouping by RateType:

~~~
      Q.GroupBy ='Type' 'RateType'
      Q.KeepEmptyGroups=0
      Q.IncludeTotalRow=1
~~~

and computing row counts and total balances:

~~~
      J=Q.AddColumn 'RowCount' 'count LoanNumber'
      J=Q.AddColumn 'TotalBalance' 'sum Balance'
      Q.Execute 0
───────────────────────────────────────────
 ┌Type──────┐  ┌RowCount┐  ┌TotalBalance─┐
 ↓Fixed     │  ↓609     │  ↓39,784,812.68│
 │Adjustable│  │641     │  │49,333,430.51│
 │          │  │1,250   │  │89,118,243.19│
 └Char(16)──┘  └Int16───┘  └Dec(2)───────┘
── 3 rows by 3 columns ────────────────────
~~~

We may now want to know the percentage each row contributes to the total, by either row count or
balance. These percentages may be computed by summing the column, and then dividing by the sum of
the sums, or the grand total:

~~~
        J=Q.AddColumn 'PctCnt' '2 round 100 * (count LoanNumber) / sum count LoanNumber'
        J=Q.AddColumn 'PctBal' '2 round 100 * (sum Balance) / sum sum Balance'
~~~

Note the slightly different expressions when computing percentages by rowcount versus a numeric
sum. We also multiply by 100 and round to 2 decimal places for presentation purposes. Let's
execute and see the result:

~~~
      Q.Execute 0
───────────────────────────────────────────────────────────────
 ┌Type──────┐  ┌RowCount┐  ┌TotalBalance─┐  ┌PctCnt┐  ┌PctBal┐
 ↓Fixed     │  ↓609     │  ↓39,784,812.68│  ↓48.72 │  ↓44.64 │
 │Adjustable│  │641     │  │49,333,430.51│  │51.28 │  │55.36 │
 │          │  │1,250   │  │89,118,243.19│  │100.00│  │100.00│
 └Char(16)──┘  └Int16───┘  └Dec(2)───────┘  └Dec(2)┘  └Dec(2)┘
── 3 rows by 5 columns ────────────────────────────────────────
~~~

If we review the Select property that yields this result, we see a fair bit of redundancy:

~~~
      Q.Select
───────────────────────────────────────────────────────────────────────────
 ┌Name────────┐  ┌Expression─────────────────────────────────────────────┐
 ↓RowCount    │  ↓count LoanNumber                                       │
 │TotalBalance│  │sum Balance                                            │
 │PctCnt      │  │2 round 100 * (count LoanNumber) / sum count LoanNumber│
 │PctBal      │  │2 round 100 * (sum Balance) / sum sum Balance          │
 └Char(12)────┘  └Char(55)───────────────────────────────────────────────┘
── 4 rows by 2 columns ────────────────────────────────────────────────────
~~~

We are counting the LoanNumber column 3 times, and summing the Balance column 3 times as well. We
can rewrite the last two expressions to take advantage of the fact that the first two expressions
have already done most of the computation:

~~~
      Q.Select=''
      J=Q.AddColumn 'RowCount' 'count LoanNumber'
      J=Q.AddColumn 'TotalBalance' 'sum Balance'
      J=Q.AddColumn 'PctCnt' '2 round 100 * RowCount / sum RowCount'
      J=Q.AddColumn 'PctBal' '2 round 100 * TotalBalance / sum TotalBalance'
~~~

And this yields the exact same result:

~~~
      Q.Execute 0
───────────────────────────────────────────────────────────────
 ┌Type──────┐  ┌RowCount┐  ┌TotalBalance─┐  ┌PctCnt┐  ┌PctBal┐
 ↓Fixed     │  ↓609     │  ↓39,784,812.68│  ↓48.72 │  ↓44.64 │
 │Adjustable│  │641     │  │49,333,430.51│  │51.28 │  │55.36 │
 │          │  │1,250   │  │89,118,243.19│  │100.00│  │100.00│
 └Char(16)──┘  └Int16───┘  └Dec(2)───────┘  └Dec(2)┘  └Dec(2)┘
── 3 rows by 5 columns ────────────────────────────────────────
~~~

Regardless of which technique above is used to compute percentages, the expression is not
particularly terse. Because this percentage calculation is extremely common, FlipDB provides the
special function →[*.percentDown] to do it directly and succinctly. The term "down" is used to
indicate that the resulting percentages are summable downward. Usually, but not always as we will
see, they will sum to 100%. The percentDown function operates on row count if its argument is
Char, or on the on the values of the argument if the argument is numeric. Thus the above two
percentage expressions may be rewritten simply as:

~~~
      Q.Select=''
      J=Q.AddColumn 'PctCnt'  '2 percentDown LoanNumber'
      J=Q.AddColumn 'PctBal'  '2 percentDown Balance'
      Q.Execute 0
──────────────────────────────────
 ┌Type──────┐  ┌PctCnt┐  ┌PctBal┐
 ↓Fixed     │  ↓48.72 │  ↓44.64 │
 │Adjustable│  │51.28 │  │55.36 │
 │          │  │100.00│  │100.00│
 └Char(16)──┘  └Dec(2)┘  └Dec(2)┘
── 3 rows by 3 columns ───────────
~~~

Note that the optional left argument to precentDown specifies how the result is to be rounded. If
omitted, the result is a Float.

Like any aggregate function, it is sometimes useful to limit the argument data, so we may ask, in
the same query, what percentage of the, say, Florida loans, by balance, are fixed or adjustable:

~~~
      J=Q.AddColumn 'FloridaOnly'  '2 percentDown Balance where State in "FL"'
      Q.Execute 0
─────────────────────────────────────────────────
 ┌Type──────┐  ┌PctCnt┐  ┌PctBal┐  ┌FloridaOnly┐
 ↓Fixed     │  ↓48.72 │  ↓44.64 │  ↓76.84      │
 │Adjustable│  │51.28 │  │55.36 │  │23.16      │
 │          │  │100.00│  │100.00│  │100.00     │
 └Char(16)──┘  └Dec(2)┘  └Dec(2)┘  └Dec(2)─────┘
── 3 rows by 4 columns ──────────────────────────
~~~

This shows that Florida has a much higher percentage of fixed rate loans than the overall
portfolio. In the context of this same report, we may also ask the question, what percentage of
the fixed rate group is in Florida? The →[*.percentAcross] function provides the answer:

~~~
      J=Q.AddColumn 'FloridaPct'  '2 percentAcross Balance (State in "FL")'
      Q.Execute 0
───────────────────────────────────────────────────────────────
 ┌Type──────┐  ┌PctCnt┐  ┌PctBal┐  ┌FloridaOnly┐  ┌FloridaPct┐
 ↓Fixed     │  ↓48.72 │  ↓44.64 │  ↓76.84      │  ↓15.27     │
 │Adjustable│  │51.28 │  │55.36 │  │23.16      │  │3.71      │
 │          │  │100.00│  │100.00│  │100.00     │  │8.87      │
 └Char(16)──┘  └Dec(2)┘  └Dec(2)┘  └Dec(2)─────┘  └Dec(2)────┘
── 3 rows by 5 columns ────────────────────────────────────────
~~~

Note well the difference between these last two columns, FloridaOnly, and FloridaPct. The
FloridaOnly column show the distribution of fixed versus adjustable collateral within Florida, and
thus is additive and sums to 100. The Florida restriction is applied to the Balance column using
the where function, which operates on the data before it is passed into the percentDown function.
The FloridaPct column, however, shows the Florida contribution to the fixed rate group and the
adjustable group, and to the total population as well. The FloridaPct percentages do not add up in
any meaningful way; they are only meaningful "across" the table, not "down" the table. Here the
Florida restriction is a direct argument to the percentAcross function. The difference can perhaps
best be seen by looking at alternative expressions for the same two columns. In the example above,
the FloridaOnly column is computed by:

~~~
      'percentDown Balance where State in "FL"'
~~~

This is equivalent to:

~~~
      '100 * (sum Balance where State in'FL')/sum sum Balance where State in "FL"'
~~~

In this expression, we see that the restriction to Florida is applied to both the numerator and
denominator, and that furthermore, the double sum in the denominator yields percentages of each
group to the whole. On the other hand, the FloridaPct column is computed by:

~~~
      'percentAcross Balance (State in "FL")'
~~~

which is equivalent to:

~~~
      '100 * (sum Balance where State in "FL") / sum Balance'
~~~

Here the restriction to Florida is applied only in the numerator, and the single sum in the
denominator yields percentages within each group.

### Generating Cross Tabulations with percentDown and percentAcross

The percentDown and percentAcross function may be used repeatedly to create cross tabulation
reports. Here the percentAcross function is used to show the distribution of occupancy status by RateType:

~~~
      Q=T.Query ''
      Q.GroupBy='Product' 'RateType'
      Q.KeepEmptyGroups=0
      Q.IncludeTotalRow=1
      J=Q.AddColumn 'Primary' '2 percentAcross Balance (OccupancyType in "Primary")'
      J=Q.AddColumn 'Second' '2 percentAcross Balance (OccupancyType in "Second")'
      J=Q.AddColumn 'Investor' '2 percentAcross Balance (OccupancyType in "Investor")'
      J=Q.AddColumn 'Total' '2 percentAcross Balance (OccupancyType in "Primary,Second,Investor")'
      Q.Execute 0
─────────────────────────────────────────────────────────
 ┌Product───┐  ┌Primary┐  ┌Second┐  ┌Investor┐  ┌Total─┐
 ↓Fixed     │  ↓91.95  │  ↓0.00  │  ↓8.05    │  ↓100.00│
 │Adjustable│  │78.95  │  │1.09  │  │19.96   │  │100.00│
 │          │  │84.75  │  │0.60  │  │14.65   │  │100.00│
 └Char(16)──┘  └Dec(2)─┘  └Dec(2)┘  └Dec(2)──┘  └Dec(2)┘
── 3 rows by 5 columns ──────────────────────────────────
~~~

Without changing the axes, we can use the percentDown function to show the distribution of rate
type by occupancy type:

~~~
       Q.Select=""
       J=Q.AddColumn 'Primary' '2 percentDown Balance where OccupancyType in "Primary"'
       J=Q.AddColumn 'Second' '2 percentDown Balance where OccupancyType in "Second"'
       J=Q.AddColumn 'Investor' '2 percentDown Balance where OccupancyType in "Investor"'
       J=Q.AddColumn 'Total' '2 percentDown Balance'
      Q.Execute 0
─────────────────────────────────────────────────────────
 ┌Product───┐  ┌Primary┐  ┌Second┐  ┌Investor┐  ┌Total─┐
 ↓Fixed     │  ↓48.43  │  ↓0.00  │  ↓24.55   │  ↓44.64 │
 │Adjustable│  │51.57  │  │100.00│  │75.45   │  │55.36 │
 │          │  │100.00 │  │100.00│  │100.00  │  │100.00│
 └Char(16)──┘  └Dec(2)─┘  └Dec(2)┘  └Dec(2)──┘  └Dec(2)┘
── 3 rows by 5 columns ──────────────────────────────────
~~~

We can also swap the axes, grouping the query by occupancy type, and writing explicit select
expressions to generate the rate type categories.

In addition to looking at the distribution of occupancy type by rate type, and rate type by
occupancy type, we can look at the overall distribution of all of the unique combinations of these
two attributes. For example, we may ask, what proportion of the portfolio is fixed-rate investor
loans? What proportion is adjustable and primary? And so on. We can do this using the →[*percentDown]
function with an additional Boolean parameter. This will foot to 100% in the bottom right corner:

~~~
      Q=T.Query ''
      Q.KeepEmptyGroups=0
      Q.GroupBy='Occupancy' 'OccupancyType'
      J=Q.AddColumn 'Fixed' '3 percentDown Balance (RateType in "Fixed")'
      J=Q.AddColumn 'Adjustable' '3 percentDown Balance (RateType in "Adjustable")'
      J=Q.AddColumn 'Total' '3 percentDown Balance'
      Q.IncludeTotalRow=1
      Q.Execute 0
────────────────────────────────────────────────
 ┌Occupancy┐  ┌Fixed─┐  ┌Adjustable┐  ┌Total──┐
 ↓Primary  │  ↓41.048│  ↓43.704    │  ↓84.752 │
 │Second   │  │0.000 │  │0.601     │  │0.601  │
 │Investor │  │3.595 │  │11.052    │  │14.647 │
 │         │  │44.643│  │55.357    │  │100.000│
 └Char(16)─┘  └Dec(3)┘  └Dec(3)────┘  └Dec(3)─┘
── 4 rows by 4 columns ─────────────────────────
~~~

Note that the boolean restriction is provided as an optional second item in the argument to the
percentDown function. This restriction is only applied to the numerator in the percentage
calculation and the denominator is not affected. Thus the expression:

~~~
      'percentDown Balance (RateType in "Fixed")'
~~~

is equivalent to:

~~~
      '100 * (sum Balance where RateType in "Fixed") / sum sum Balance'
~~~

### Generating Cross Tabulations with Multiple Groupings

In the above cross tab examples the vertical axis of the query result is generated by the unique
values of a particular column while the horizontal axis is generated by multiple calls to the
percentDown function, each with an explicit Boolean restriction to define a group. It is possible
to generate exactly the same set of statistics, in a different form, using multiple grouping
levels in the query. In this technique, the groupings of both dimensions are defined by the data:

~~~
      Q=T.Query ''
      Q.KeepEmptyGroups=0
      Q.IncludeTotalRow=1
      J=Q.AddGroupBy 'Product' 'RateType'
      J=Q.AddGroupBy 'Occupancy' 'OccupancyType'
      J=Q.AddColumn 'RowCount' 'count LoanNumber'
      J=Q.AddColumn 'TotalBalance' 'sum Balance'
      J=Q.AddColumn 'Percent' '3 percentDown Balance'
      Q.Execute 0
───────────────────────────────────────────────────────────────────
 ┌Product───┐  ┌Occupancy┐  ┌RowCount┐  ┌TotalBalance─┐  ┌Percent┐
 ↓Fixed     │  ↓Primary  │  ↓534     │  ↓36,580,896.27│  ↓41.048 │
 │Fixed     │  │Investor │  │75      │  │3,203,916.41 │  │3.595  │
 │Adjustable│  │Primary  │  │467     │  │38,948,278.80│  │43.704 │
 │Adjustable│  │Second   │  │12      │  │535,923.72   │  │0.601  │
 │Adjustable│  │Investor │  │162     │  │9,849,227.99 │  │11.052 │
 │          │  │         │  │1,250   │  │89,118,243.19│  │100.000│
 └Char(16)──┘  └Char(16)─┘  └Int16───┘  └Dec(2)───────┘  └Dec(3)─┘
── 6 rows by 5 columns ────────────────────────────────────────────
~~~


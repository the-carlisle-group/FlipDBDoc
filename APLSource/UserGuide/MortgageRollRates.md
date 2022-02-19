# Mortgage Roll Rates

The delinquency status of a mortgage changes over time. A mortgage may be current in January, one
month delinquent in February, 2 months delinquent in March, and then current again April.  For any
given month we may ask "how many loans that were current last month are now 30 days delinquent?" Or
how many loans that were 90 days late are now current? These statistics are known as "roll rates",
as a mortgage will (possibly) roll from one category to another each month. Let's assume five
categories for the loan status: current and 30, 60, 90, and 120+ days delinquent. (You could
easily have more, say in forcelosure and REO (real estate owned). With 5 categories, there will be
5x5, or 25, potential roll rates for a given month. However, some roll rates are not applicable.
For example, a 30 day delinquent loan may roll current, stay the same, or go 60 days late, but it
cannot go 90 or 120 days late in one period. If the roll rates are displayed as a cross-tabulation
with the previous months on the x-axis, and the current month on the y-axis, the upper right
corner will be not applicable.

In the following discussion we will use the built-in sample loan database, generated with 10,000
loans and 18 months of payment history:

~~~
      S=Scripting.Server
      J=S.DeleteDatabase 'SampleLoan'
      D=S.BuildSampleLoan 'SampleLoan' 10000 18
~~~

The PaymentHistory table has a "LoanStatus" column that indicates whether the loan is current, or
delinquent. In general, this column is computed by inspecting the difference between the AsOfDate
and the PaidThruDate columns. We can see the distribution of this column:

~~~
      PT=D.GetTable 'PaymentHistory'
      Q=PT.Query ''
      Q.GroupBy='LoanStatus'
      Q.Execute 0
─────────────────────────────────
 ┌GroupByLoanStatus┐  ┌RowCount┐
 ↓Current          │  ↓164,181 │
 │Delq30           │  │13,401  │
 │Delq60           │  │465     │
 │Delq90           │  │0       │
 │Delq120          │  │0       │
 │                 │  │0       │
 └Char(16)─────────┘  └Int32───┘
── 6 rows by 2 columns ──────────
~~~

In general, when reporting on roll rates from the payment history table you must break out by
AsOfDate, either by selecting a specific AsOFDate in the where clause or by adding AsOfDate as an
extra level of grouping. Here we select a specific month:

~~~
      Q=PT.Query ''
      Q.Where='AsOfDate eq "2004/4/1"'
      J=Q.GroupBy='LoanStatus'
      Q.Execute 0
─────────────────────────────────
 ┌GroupByLoanStatus┐  ┌RowCount┐
 ↓n/a              │  ↓0       │
 │Current          │  │7,280   │
 │Delq30           │  │1,360   │
 │Delq60           │  │83      │
 │Delq90           │  │3       │
 │Delq120          │  │0       │
 └Char(16)─────────┘  └Int16───┘
── 6 rows by 2 columns ──────────
~~~

Note the inclusion of empty groups. These show up because the LoanStatus column is a foreign key
referencing the LoanStatus table, which maintains a complete, ordered list of status values:

~~~
     D.GetTable 'LoanStatus'
── SampleLoan.LoanStatus
 ┌LoanStatus┐  ┌Code───┐
 ↓n/a       │  ↓n/a    │
 │Current   │  │C      │
 │Delq30    │  │D30    │
 │Delq60    │  │D60    │
 │Delq90    │  │D90    │
 │Delq120   │  │D120   │
 └Char(16)──┘  └Char(4)┘
── 6 rows by 2 columns ──
~~~

It is important that the LoanStatus column in the Loan table is a foreign key so that queries
grouped by it will return a known number of rows regardless of the actual data.

To compute roll rates, we need to compare the previous status with the current status for each row.
It is best to compute a new column, so that we may specify it as a foreign key:

~~~
      C=PT.Evaluate 'lag partitionBy (LoanStatus 1 "n/a") LoanNumber (AsOfDate "Asc")'
      PLS=PT.AddColumn 'PreviousLoanStatus' C
      J=PLS.SetForeignKey "LoanStatus"
~~~

The expression for "PreviousLoanStatus" involves shifting the LoanStats column within unique loan
numbers, ordered by AsOfDate. There will necessarily be a set of loans that have no value and will
be assigned "n/a". Note that one reason for pre-calculating this column, rather than embedding the
expression in the query, is that the lag function is row dependent, and thus dependent on the
where clause.

Now we are ready to construct a cross tab query comparing previous loan status with the current
loan status:

~~~
      Q=PT.Query ''
      Q.Where='AsOfDate == "2004/4/1"'
      Q.GroupBy='PreviousLoanStatus'
      J=Q.AddColumn 'Current' 'sum LoanStatus in "Current"'
      J=Q.AddColumn 'Delq30' 'sum LoanStatus in "Delq30"'
      J=Q.AddColumn 'Delq60' 'sum LoanStatus in "Delq60"'
      J=Q.AddColumn 'Delq90' 'sum LoanStatus in "Delq90"'
      J=Q.AddColumn 'Delq120' 'sum LoanStatus in "Delq120"'
      Q.Execute 0
─────────────────────────────────────────────────────────────────────────────────
 ┌GroupByPreviousLoanStatus┐  ┌Current┐  ┌Delq30┐  ┌Delq60┐  ┌Delq90┐  ┌Delq120┐
 ↓n/a                      │  ↓0      │  ↓0     │  ↓0     │  ↓0     │  ↓0      │
 │Current                  │  │7,271  │  │58    │  │0     │  │0     │  │0      │
 │Delq30                   │  │8      │  │1,302 │  │11    │  │0     │  │0      │
 │Delq60                   │  │1      │  │0     │  │71    │  │0     │  │0      │
 │Delq90                   │  │0      │  │0     │  │1     │  │3     │  │0      │
 │Delq120                  │  │0      │  │0     │  │0     │  │0     │  │0      │
 └Char(16)─────────────────┘  └Int16──┘  └Int16─┘  └Int8──┘  └Int8──┘  └Boolean┘
── 6 rows by 6 columns ──────────────────────────────────────────────────────────
~~~

This may be modified by computing percentages rather than loan counts, and run on the entire
portfolio by adding an extra level of grouping:

~~~
      Q=PT.Query ''
       Q.GroupBy='AsOfDate'
       J=Q.AddGroupBy 'PLS' 'PreviousLoanStatus'
       J=Q.AddColumn 'Count' 'count LoanNumber'
       J=Q.AddColumn 'Current' '2 round percentAcross LoanNumber (LoanStatus in "Current")'
       J=Q.AddColumn 'Delq30' '2 round percentAcross LoanNumber (LoanStatus in "Delq30")'
       J=Q.AddColumn 'Delq60' '2 round percentAcross LoanNumber (LoanStatus in "Delq60")'
       J=Q.AddColumn 'Delq90' '2 round percentAcross LoanNumber (LoanStatus in "Delq90")'
       J=Q.AddColumn 'Delq120' '2 round percentAcross LoanNumber (LoanStatus in "Delq120")'
       Q.Execute 0
─────────────────────────────────────────────────────────────────────────────────────────────
 ┌GroupByAsOfDate┐  ┌PLS─────┐  ┌Count─┐  ┌Current┐  ┌Delq30┐  ┌Delq60┐  ┌Delq90┐  ┌Delq120┐
 ↓2002-10-01     │  ↓n/a     │  ↓10,000│  ↓100.00 │  ↓0.00  │  ↓0.00  │  ↓0.00  │  ↓0.00   │
 │2002-10-01     │  │Current │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-10-01     │  │Delq30  │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-10-01     │  │Delq60  │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-10-01     │  │Delq90  │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-10-01     │  │Delq120 │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-11-01     │  │n/a     │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-11-01     │  │Current │  │10,000│  │98.90  │  │1.10  │  │0.00  │  │0.00  │  │0.00   │
 │2002-11-01     │  │Delq30  │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-11-01     │  │Delq60  │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-11-01     │  │Delq90  │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-11-01     │  │Delq120 │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-12-01     │  │n/a     │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-12-01     │  │Current │  │9,811 │  │98.79  │  │1.21  │  │0.00  │  │0.00  │  │0.00   │
 │2002-12-01     │  │Delq30  │  │110   │  │0.91   │  │98.18 │  │0.91  │  │0.00  │  │0.00   │
 │2002-12-01     │  │Delq60  │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-12-01     │  │Delq90  │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2002-12-01     │  │Delq120 │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2003-01-01     │  │n/a     │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2003-01-01     │  │Current │  │9,627 │  │99.15  │  │0.85  │  │0.00  │  │0.00  │  │0.00   │
 │2003-01-01     │  │Delq30  │  │225   │  │0.89   │  │97.78 │  │1.33  │  │0.00  │  │0.00   │
 │2003-01-01     │  │Delq60  │  │1     │  │0.00   │  │0.00  │  │100.00│  │0.00  │  │0.00   │
 │2003-01-01     │  │Delq90  │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2003-01-01     │  │Delq120 │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │2003-02-01     │  │n/a     │  │0     │  │0.00   │  │0.00  │  │0.00  │  │0.00  │  │0.00   │
 │...            │  │...     │  │...   │  │...    │  │...   │  │...   │  │...   │  │...    │
 └Date───────────┘  └Char(16)┘  └Int16─┘  └Dec(2)─┘  └Dec(2)┘  └Dec(2)┘  └Dec(2)┘  └Dec(2)─┘
── 114 rows by 8 columns ────────────────────────────────────────────────────────────────────
~~~

It is also possible to analyze roll rates starting from the loan table. In this case there is no
need to shift the LoanStatus column with the lag function as FlipDB allows us to directly access
time series data for a particular period:

~~~
      LT=D.GetTable "Loan"
      LQ=LT.Query ''
      J=LQ.AddGroupBy 'PreviousLoanStatus' 'PaymentHistory[2004/03/01].LoanStatus'
      J= LQ.AddGroupBy 'CurrentLoanStatus' 'PaymentHistory[2004/04/01].LoanStatus'
      LQ.KeepEmptyGroups=0
      LQ.Having='not CurrentLoanStatus eq ""'
      LQ.Execute 0
───────────────────────────────────────────────────────
 ┌PreviousLoanStatus┐  ┌CurrentLoanStatus┐  ┌RowCount┐
 ↓Current           │  ↓Current          │  ↓7,394   │
 │Current           │  │Delq30           │  │93      │
 │Delq30            │  │Current          │  │13      │
 │Delq30            │  │Delq30           │  │1,216   │
 │Delq30            │  │Delq60           │  │2       │
 │Delq60            │  │Delq60           │  │80      │
 │Delq60            │  │Delq90           │  │1       │
 │Delq90            │  │Delq90           │  │1       │
 └Char(16)──────────┘  └Char(16)─────────┘  └Int16───┘
── 8 rows by 3 columns ────────────────────────────────
~~~


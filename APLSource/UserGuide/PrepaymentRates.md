# Mortgage Prepayment Rates: CPR

It is customary to compute CPRs over 1, 3, 6, and 12 month time periods.

Computing historical prepayment rates over a time period requires the actual balance of a pool of
loans at the end of the period, and the scheduled balance, relative to the period, of the same
pool of loans In other words, we want to compare the balance as it actually is at the end of the
period, with the balance that we thought we would have at the end of the period, given the balance
at the beginning of the period. The actual balance at the end of the period reflects any
prepayments that have occurred over the time period, while the scheduled balance does not.

For example, let's  say we want to compute a 3 month CPR for the period ending 4/1/2010. The first
thing we need to know is the actual balance on April 1st, 2010. Then we need to know the actual
balance as it was 3 months earlier, on January 1st. This previous balance may then be "rolled"
forward 3 months, computing the scheduled balance for April 1st. Now we have two balances to compare.

In the following discussion we will use the built-in sample loan database, generated with 10,000
loans and 18 months of payment history:

~~~
      S=Scripting.Server
      J=S.DeleteDatabase 'SampleLoan'
      D=S.BuildSampleLoan 'SampleLoan' 10000 18
~~~

The PaymentHistory table records balances over time. Each month, this table is appended with the
current balances, rates, monthly payments paid thru dates, and perhaps other time-sensitive data,
for each loan in the Loan table. Given the current balance, rate and payment, we can easily roll
the balance forward 1, 3, 6 and 12 months, with the following expressions:

~~~
      PT=D.GetTable 'PaymentHistory'
      Q=PT.Query ''
      J=Q.AddColumn 'SB1M' '2 round rollBalance Balance Rate PandI 1'
      J=Q.AddColumn 'SB3M' '2 round rollBalance Balance Rate PandI 3'
      J=Q.AddColumn 'SB6M' '2 round rollBalance Balance Rate PandI 6'
      J=Q.AddColumn 'SB12M' '2 round rollBalance Balance Rate PandI 12'
      Q.Select
────────────────────────────────────────────────────────
 ┌Name───┐  ┌Expression───────────────────────────────┐
 ↓SB1M   │  ↓2 round rollBalance Balance Rate PandI 1 │
 │SB3M   │  │2 round rollBalance Balance Rate PandI 3 │
 │SB6M   │  │2 round rollBalance Balance Rate PandI 6 │
 │SB12M  │  │2 round rollBalance Balance Rate PandI 12│
 └Char(5)┘  └Char(41)─────────────────────────────────┘
── 4 rows by 2 columns ─────────────────────────────────
~~~

Note that the sample PaymentHistory table comes with these columns, but that in a real-world
scenario they would probably have to be calculated using the above expressions. Note further that
these expressions assume the loan is not scheduled to mature within the period. From the sample
PaymentHistory table:

~~~
      Q=PT.Query ''
      Q.Where = 'LoanNumber in "00000001"'
      Q.Select='LoanNumber,AsOfDate,PaidThruDate,Balance,SB1M,SB3M'
      Q.WhereQuota=5
      Q.Execute 0
────────────────────────────────────────────────────────────────────────────────
 ┌LoanNumber┐  ┌AsOfDate──┐  ┌PaidThruDate┐  ┌Balance─┐  ┌SB1M────┐  ┌SB3M────┐
 ↓00000001  │  ↓2002-10-01│  ↓2002-10-01  │  ↓35154.79│  ↓35119.71│  ↓35048.94│
 │00000001  │  │2002-11-01│  │2002-11-01  │  │35119.70│  │35084.42│  │35013.24│
 │00000001  │  │2002-12-01│  │2002-12-01  │  │35084.41│  │35048.92│  │34977.34│
 │00000001  │  │2003-01-01│  │2003-01-01  │  │35048.92│  │35013.23│  │34941.24│
 │00000001  │  │2003-02-01│  │2003-02-01  │  │35013.23│  │34977.34│  │34904.93│
 └Char(16)──┘  └Date──────┘  └Date────────┘  └Dec(2)──┘  └Dec(2)──┘  └Dec(2)──┘
── 5 rows by 6 columns ─────────────────────────────────────────────────────────
~~~

Note that these computed balances are future scheduled balances. For example, SB3M is the scheduled
balance 3 months from the as of date on the same row. To compute CPRs, we must shift the scheduled
balance columns down by the appropriate number of monthly periods.  The first approach to
computing CPRs is to start in the Payment history table, group by AsOfDate, and calculate CPRs
over time, by using the lag function:

~~~
      Q=PT.Query ''
      Q.GroupBy='AsOfDate'
      J=Q.AddColumn 'Count' 'count LoanNumber'
      J=Q.AddColumn 'TotalBalance' 'sum Balance'
      J=Q.AddColumn 'CPR1M' '2 round 100 times 1 - (TotalBalance divide lag (sum SB1M) 1 0) power 12'
      J=Q.AddColumn 'CPR3M' '2 round 100 times 1 - (TotalBalance divide lag (sum SB3M) 3 0) power 4'
      J=Q.AddColumn 'CPR6M' '2 round 100 times 1 - (TotalBalance divide lag (sum SB6M) 6 0) power 2'
      J=Q.AddColumn 'CPR12M' '2 round 100 times 1 - TotalBalance divide lag (sum SB12M) 12 0'
      Q.Execute 0
────────────────────────────────────────────────────────────────────────────────────
 ┌GroupByAsOfDate┐  ┌Count┐  ┌TotalBalance┐  ┌CPR1M─┐  ┌CPR3M─┐  ┌CPR6M─┐  ┌CPR12M┐
 ↓2002-10-01     │  ↓10000│  ↓715127707.00│  ↓100.00│  ↓100.00│  ↓100.00│  ↓100.00│
 │2002-11-01     │  │10000│  │708020422.88│  │  9.35│  │100.00│  │100.00│  │100.00│
 │2002-12-01     │  │ 9912│  │700905209.82│  │  9.39│  │100.00│  │100.00│  │100.00│
 │2003-01-01     │  │ 9843│  │695569156.11│  │  6.66│  │  8.52│  │100.00│  │100.00│
 │2003-02-01     │  │ 9774│  │688012159.54│  │ 10.26│  │  8.77│  │100.00│  │100.00│
 │2003-03-01     │  │ 9687│  │680132848.52│  │ 10.86│  │  9.28│  │100.00│  │100.00│
 │2003-04-01     │  │ 9600│  │673060701.90│  │  9.71│  │ 10.29│  │  9.44│  │100.00│
 │2003-05-01     │  │ 9515│  │664281393.96│  │ 12.57│  │ 11.04│  │  9.91│  │100.00│
 │2003-06-01     │  │ 9441│  │657096548.33│  │ 10.14│  │ 10.81│  │ 10.05│  │100.00│
 │2003-07-01     │  │ 9353│  │649878820.25│  │ 10.31│  │ 11.02│  │ 10.65│  │100.00│
 │2003-08-01     │  │ 9276│  │645546731.59│  │  5.48│  │  8.67│  │  9.85│  │100.00│
 │2003-09-01     │  │ 9206│  │637803071.27│  │ 11.41│  │  9.09│  │  9.94│  │100.00│
 │2003-10-01     │  │ 9140│  │632383393.41│  │  7.52│  │  8.15│  │  9.60│  │  9.54│
 │2003-11-01     │  │ 9063│  │624471206.61│  │ 11.90│  │ 10.32│  │  9.48│  │  9.68│
 │2003-12-01     │  │ 8980│  │618882770.52│  │  7.96│  │  9.16│  │  9.11│  │  9.57│
 │2004-01-01     │  │ 8921│  │612585010.66│  │  9.33│  │  9.75│  │  8.95│  │  9.80│
 │2004-02-01     │  │ 8849│  │607258332.29│  │  7.66│  │  8.30│  │  9.35│  │  9.56│
 │2004-03-01     │  │ 8767│  │597252971.13│  │ 15.99│  │ 11.07│  │ 10.12│  │ 10.02│
 │2004-04-01     │  │ 8686│  │593613632.25│  │  4.62│  │  9.56│  │  9.66│  │  9.63│
 └Date───────────┘  └Int16┘  └Dec(2)──────┘  └Dec(2)┘  └Dec(2)┘  └Dec(2)┘  └Dec(2)┘
── 19 rows by 7 columns ────────────────────────────────────────────────────────────
~~~

In this technique, the sum of current or actual balance and the sum of the future scheduled
balances are computed for each as-of date. The future scheduled balances sums must then be shifted
down, or lagged, by the appropriate number of periods, in order line up with the appropriate
actual balance. Note that this sum-then-lag technique is only applicable when grouping by
AsOFDate. The CPRs are annualized by raising to the 12/n, where n is the number of months in the
period. This exponent is 1 in the case of 12 month CPR, and thus falls out of the equation. When
the scheduled balances columns are lagged, the fill value is 0. This results in CPRs of 100 for
non-applicable results. The toChar function with a formatting string may be used to present the
output in a more suitable form. Here the non-applicable 12 month CPRs are specified as "n/a",
while the 6 month CPRs are blank:

~~~
      Q=PT.Query ''
      Q.GroupBy='AsOfDate'
      J=Q.AddColumn 'Count' 'count LoanNumber'
      J=Q.AddColumn 'TotalBalance' 'sum Balance'
      J=Q.AddColumn 'CPR6M' '"O100<>F10.2" toChar 2 round 100 times 1 - (TotalBalance divide lag (sum SB6M) 6 0) power 2'
      J=Q.AddColumn 'CPR12M' '"O100<n/a>F10.2" toChar 2 round 100 times 1 - TotalBalance divide lag (sum SB12M) 12 0'
      Q.Execute 0
────────────────────────────────────────────────────────────────────────
 ┌GroupByAsOfDate┐  ┌Count┐  ┌TotalBalance┐  ┌CPR6M─────┐  ┌CPR12M────┐
 ↓2002-10-01     │  ↓10000│  ↓715127707.00│  ↓          │  ↓       n/a│
 │2002-11-01     │  │10000│  │708020422.88│  │          │  │       n/a│
 │2002-12-01     │  │ 9912│  │700905209.82│  │          │  │       n/a│
 │2003-01-01     │  │ 9843│  │695569156.11│  │          │  │       n/a│
 │2003-02-01     │  │ 9774│  │688012159.54│  │          │  │       n/a│
 │2003-03-01     │  │ 9687│  │680132848.52│  │          │  │       n/a│
 │2003-04-01     │  │ 9600│  │673060701.90│  │      9.44│  │       n/a│
 │2003-05-01     │  │ 9515│  │664281393.96│  │      9.91│  │       n/a│
 │2003-06-01     │  │ 9441│  │657096548.33│  │     10.05│  │       n/a│
 │2003-07-01     │  │ 9353│  │649878820.25│  │     10.65│  │       n/a│
 │2003-08-01     │  │ 9276│  │645546731.59│  │      9.85│  │       n/a│
 │2003-09-01     │  │ 9206│  │637803071.27│  │      9.94│  │       n/a│
 │2003-10-01     │  │ 9140│  │632383393.41│  │      9.60│  │      9.54│
 │2003-11-01     │  │ 9063│  │624471206.61│  │      9.48│  │      9.68│
 │2003-12-01     │  │ 8980│  │618882770.52│  │      9.11│  │      9.57│
 │2004-01-01     │  │ 8921│  │612585010.66│  │      8.95│  │      9.80│
 │2004-02-01     │  │ 8849│  │607258332.29│  │      9.35│  │      9.56│
 │2004-03-01     │  │ 8767│  │597252971.13│  │     10.12│  │     10.02│
 │2004-04-01     │  │ 8686│  │593613632.25│  │      9.66│  │      9.63│
 └Date───────────┘  └Int16┘  └Dec(2)──────┘  └Char(10)──┘  └Char(10)──┘
── 19 rows by 5 columns ────────────────────────────────────────────────
~~~

Now consider computing CPRs as of a particular date, for various breakouts of the portfolio. For
example, consider computing CPRs by RateType (fixed or ARM) for the most recent data, in this case
4/1/2004. To accomplish this we start in the Loan table rather than the PaymentHistory table:

~~~
      LT=D.GetTable 'Loan'
      Q=LT.Query ''
      Q.GroupBy="RateType"
      Q.KeepEmptyGroups=0
      Q.IncludeTotalRow=1
      J=Q.AddColumn 'CurrentBalance' 'PaymentHistory[0].Balance' 0
      J=Q.AddColumn 'Active' 'PaymentHistory[0].AsOfDate ne 0' 0
      J=Q.AddColumn 'NumberOfLoans' 'count LoanNumber where Active'
      J=Q.AddColumn 'TotalBalance' 'sum CurrentBalance where Active'
      J=Q.AddColumn 'CPR1M' '2 round 100 times 1 - (TotalBalance divide sum PaymentHistory[-1].SB1M) power 12'
      J=Q.AddColumn 'CPR3M' '2 round 100 times 1 - (TotalBalance divide sum PaymentHistory[-3].SB3M) power 4'
      J=Q.AddColumn 'CPR6M' '2 round 100 times 1 - (TotalBalance divide sum PaymentHistory[-6].SB6M) power 2'
      J=Q.AddColumn 'CPR12M' '2 round 100 times 1 - TotalBalance divide sum PaymentHistory[-12].SB12M'
      Q.Execute 0
────────────────────────────────────────────────────────────────────────────────────────────
 ┌GroupByRateType┐  ┌NumberOfLoans┐  ┌TotalBalance┐  ┌CPR1M─┐  ┌CPR3M─┐  ┌CPR6M─┐  ┌CPR12M┐
 ↓Fixed          │  ↓4266         │  ↓265623242.92│  ↓10.98 │  ↓11.17 │  ↓10.68 │  ↓10.17 │
 │Adjustable     │  │4420         │  │327990389.33│  │-0.89 │  │ 8.23 │  │ 8.82 │  │ 9.18 │
 │               │  │8686         │  │593613632.25│  │ 4.62 │  │ 9.56 │  │ 9.66 │  │ 9.63 │
 └Char(16)───────┘  └Int16────────┘  └Dec(2)──────┘  └Dec(2)┘  └Dec(2)┘  └Dec(2)┘  └Dec(2)┘
── 3 rows by 7 columns ─────────────────────────────────────────────────────────────────────
~~~

The first expression for "CurrentBalance" simply accesses the most recent balance from the payment
history table. Note the trailing 0 after the expression. This indicates that this column should
not be returned in the result, but will be accessible for other expressions in the query, as a
temporary column. The second expression computes a boolean flag to indicate active loans; a loan
is considered active on a particular date if there is a corresponding row in the payment history
table for that date. The loan count and total current balance is then computed for the grouping.
In order to compute the 6 month CPRs we need the 6 month scheduled balances for each loan as they
were months previously. This is provided by the expression:

~~~
PaymentHistory[-6].SB6M
~~~

This expression reaches into the PaymentHistory table and extracts the column SB6M where the
AsOfDate is 6 months earlier than the most recent AsOfDate. (Note the negative sign indicates that
that the 6 represents months; A positive value of 6 would look back 6 days, which is not
appropriate for mortgage data.) The effect of this expression is to "lag" the column SB6M so that
the appropriate 6-month scheduled balances line up with the current balances and may be directly
compared to compute the 6-month CPRs for each group. Note that in the example above where the
query started in the PaymentHistory table, the scheduled balances where first summed and then
lagged (and the lagging is accomplished via the lag function), while in this example, starting in
the Loan table, the scheduled balances are lagged and then summed(and the lagging is accomplished
by the bracket notation).

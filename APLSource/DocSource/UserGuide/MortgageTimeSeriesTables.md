# Mortgage Time Series Data

The state of a mortgage changes every month, as balances are paid down. A mortgage may also pay off
completely, or go delinquent, or into foreclosure. In addition, if the mortgage is has an
adjustable rate, the rate and scheduled monthly payment may change. In short, if we want to
analyze a pool of mortgages over time, we must have monthly information on a number of columns.
Usually this monthly information is accumulated over time from a mortgage servicer. This payment
history data must be stored in a separate table from the static loan data, as there will be many
payment history rows for each loan. With the data properly arranged in these two tables, it is
easy to report on prepayment rates and delinquency roll rates over time or for various cohorts of
the current portfolio. FlipDB has a built-in method that creates a sample mortgage database, with
a Loan table to hold the static data, and a PaymentHistory to hold the monthly data, as well as
other ancillary lookup tables. We can use this to create a database with 10,000 loans and 18
months of payment history:

~~~
      S=Scripting.Server
      J=S.DeleteDatabase 'SampleLoan'
      D=S.BuildSampleLoan 'SampleLoan' 10000 18
~~~

The Loan table contains static loan data:

~~~
      LT=D.GetTable 'Loan'
      LQ=LT.Query ''
      LQ.WhereQuota=10
      LQ.Select = 'LoanNumber,OriginalBalance,OccupancyType,State'
      LQ.Execute 0
── Key:LoanNumber ───────────────────────────────────────────
 ┌LoanNumber┐  ┌OriginalBalance┐  ┌OccupancyType┐  ┌State──┐
 ↓00000001  │  ↓36000.00       │  ↓Investor     │  ↓VA     │
 │00000002  │  │31500.00       │  │Investor     │  │VA     │
 │00000003  │  │60000.00       │  │Primary      │  │VA     │
 │00000004  │  │77800.00       │  │Primary      │  │VA     │
 │00000005  │  │40700.00       │  │Investor     │  │GA     │
 │00000006  │  │71700.00       │  │Primary      │  │VA     │
 │00000007  │  │62550.00       │  │Primary      │  │VA     │
 │00000008  │  │69356.00       │  │Primary      │  │VA     │
 │00000009  │  │38500.00       │  │Primary      │  │GA     │
 │00000010  │  │42850.00       │  │Primary      │  │GA     │
 └Char(16)──┘  └Dec(2)─────────┘  └Char(16)─────┘  └Char(2)┘
── 10 rows by 4 columns ─────────────────────────────────────
~~~

For each loan in the Loan table, there are multiple rows in the PaymentHistory table. Here is the
payment history for loan number "00000003":

~~~
      PT=D.GetTable 'PaymentHistory'
      PQ=PT.Query ''
      PQ.Where='LoanNumber in "00000003"'
      PQ.Select='LoanNumber,AsOfDate,PaidThruDate,Balance,Rate,PandI'
      PQ.Execute 0
────────────────────────────────────────────────────────────────────────────
 ┌LoanNumber┐  ┌AsOfDate──┐  ┌PaidThruDate┐  ┌Balance─┐  ┌Rate──┐  ┌PandI─┐
 ↓00000003  │  ↓2002-10-01│  ↓2002-10-01  │  ↓58770.21│  ↓7.375 │  ↓414.41│
 │00000003  │  │2002-11-01│  │2002-11-01  │  │58716.99│  │7.375 │  │414.41│
 │00000003  │  │2002-12-01│  │2002-12-01  │  │58663.44│  │7.375 │  │414.41│
 │00000003  │  │2003-01-01│  │2003-01-01  │  │58609.56│  │7.375 │  │414.41│
 │00000003  │  │2003-02-01│  │2003-02-01  │  │58555.35│  │7.375 │  │414.41│
 │00000003  │  │2003-03-01│  │2003-03-01  │  │58500.81│  │7.375 │  │414.41│
 │00000003  │  │2003-04-01│  │2003-04-01  │  │58445.93│  │7.375 │  │414.41│
 │00000003  │  │2003-05-01│  │2003-05-01  │  │58390.71│  │7.375 │  │414.41│
 │00000003  │  │2003-06-01│  │2003-06-01  │  │58335.15│  │7.375 │  │414.41│
 │00000003  │  │2003-07-01│  │2003-07-01  │  │58279.25│  │7.375 │  │414.41│
 │00000003  │  │2003-08-01│  │2003-08-01  │  │58223.01│  │7.375 │  │414.41│
 │00000003  │  │2003-09-01│  │2003-09-01  │  │58166.42│  │7.375 │  │414.41│
 │00000003  │  │2003-10-01│  │2003-10-01  │  │58109.49│  │7.375 │  │414.41│
 │00000003  │  │2003-11-01│  │2003-11-01  │  │58052.21│  │7.375 │  │414.41│
 │00000003  │  │2003-12-01│  │2003-12-01  │  │57994.57│  │7.375 │  │414.41│
 │00000003  │  │2004-01-01│  │2004-01-01  │  │57936.58│  │7.375 │  │414.41│
 │00000003  │  │2004-02-01│  │2004-02-01  │  │57878.23│  │7.375 │  │414.41│
 │00000003  │  │2004-03-01│  │2004-03-01  │  │57819.52│  │7.375 │  │414.41│
 │00000003  │  │2004-04-01│  │2004-04-01  │  │57760.45│  │7.375 │  │414.41│
 └Char(16)──┘  └Date──────┘  └Date────────┘  └Dec(2)──┘  └Dec(3)┘  └Dec(2)┘
── 19 rows by 6 columns ────────────────────────────────────────────────────
~~~

The PaymentHistory table has a foreign key column "LoanNumber" that refers to the primary key of
the Loan table, also named "LoanNumber". In addition, the PaymentHistory has a column named
"AsOfDate", which by convention in FlipDB is the default time series column. Given these columns
and relationships we can easily start in either table and reference columns from the other table.
For example, starting in the Loan table, we can get the 4/1 balances from the Payment History table:

~~~
      J=LQ.AddColumn 'CurrentBalance' 'PaymentHistory[2004/4/1].Balance'
      LQ.Execute 0
── Key:LoanNumber ─────────────────────────────────────────────────────────────
 ┌LoanNumber┐  ┌OriginalBalance┐  ┌OccupancyType┐  ┌State──┐  ┌CurrentBalance┐
 ↓00000001  │  ↓36000.00       │  ↓Investor     │  ↓VA     │  ↓34491.49      │
 │00000002  │  │31500.00       │  │Investor     │  │VA     │  │30213.84      │
 │00000003  │  │60000.00       │  │Primary      │  │VA     │  │57760.45      │
 │00000004  │  │77800.00       │  │Primary      │  │VA     │  │74651.83      │
 │00000005  │  │40700.00       │  │Investor     │  │GA     │  │38909.31      │
 │00000006  │  │71700.00       │  │Primary      │  │VA     │  │70230.57      │
 │00000007  │  │62550.00       │  │Primary      │  │VA     │  │61432.33      │
 │00000008  │  │69356.00       │  │Primary      │  │VA     │  │66488.90      │
 │00000009  │  │38500.00       │  │Primary      │  │GA     │  │37131.01      │
 │00000010  │  │42850.00       │  │Primary      │  │GA     │  │    0.00      │
 └Char(16)──┘  └Dec(2)─────────┘  └Char(16)─────┘  └Char(2)┘  └Dec(2)────────┘
── 10 rows by 5 columns ───────────────────────────────────────────────────────
~~~

And starting in the PaymentHistory table, we can get the occupancy type from the loan table:

~~~
      J=PQ.AddColumn 'OccupancyType' 'LoanNumber.OccupancyType'
      PQ.Execute 0
─────────────────────────────────────────────────────────────────────────────────────────────
 ┌LoanNumber┐  ┌AsOfDate──┐  ┌PaidThruDate┐  ┌Balance─┐  ┌Rate──┐  ┌PandI─┐  ┌OccupancyType┐
 ↓00000003  │  ↓2002-10-01│  ↓2002-10-01  │  ↓58770.21│  ↓7.375 │  ↓414.41│  ↓Primary      │
 │00000003  │  │2002-11-01│  │2002-11-01  │  │58716.99│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2002-12-01│  │2002-12-01  │  │58663.44│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-01-01│  │2003-01-01  │  │58609.56│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-02-01│  │2003-02-01  │  │58555.35│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-03-01│  │2003-03-01  │  │58500.81│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-04-01│  │2003-04-01  │  │58445.93│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-05-01│  │2003-05-01  │  │58390.71│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-06-01│  │2003-06-01  │  │58335.15│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-07-01│  │2003-07-01  │  │58279.25│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-08-01│  │2003-08-01  │  │58223.01│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-09-01│  │2003-09-01  │  │58166.42│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-10-01│  │2003-10-01  │  │58109.49│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-11-01│  │2003-11-01  │  │58052.21│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2003-12-01│  │2003-12-01  │  │57994.57│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2004-01-01│  │2004-01-01  │  │57936.58│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2004-02-01│  │2004-02-01  │  │57878.23│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2004-03-01│  │2004-03-01  │  │57819.52│  │7.375 │  │414.41│  │Primary      │
 │00000003  │  │2004-04-01│  │2004-04-01  │  │57760.45│  │7.375 │  │414.41│  │Primary      │
 └Char(16)──┘  └Date──────┘  └Date────────┘  └Dec(2)──┘  └Dec(3)┘  └Dec(2)┘  └Char(16)─────┘
── 19 rows by 7 columns ─────────────────────────────────────────────────────────────────────
~~~

From the PaymentHistory table, we can look at the portfolio over time by grouping by AsOfDate,
bringing in static data from the Loan table where needed:

~~~
      Q=PT.Query ''
      Q.GroupBy = 'AsOfDate'
      J=Q.AddColumn 'LoanCount' 'count LoanNumber'
      J=Q.AddColumn 'TotalBalance' 'sum Balance'
      J=Q.AddColumn 'PercentPrimary' '2 round percentAcross Balance (LoanNumber.OccupancyType in "Primary")'
      Q.Execute 0
──────────────────────────────────────────────────────────────────
 ┌GroupByAsOfDate┐  ┌LoanCount┐  ┌TotalBalance┐  ┌PercentPrimary┐
 ↓2002-10-01     │  ↓10000    │  ↓715127707.00│  ↓84.67         │
 │2002-11-01     │  │10000    │  │708020422.88│  │84.76         │
 │2002-12-01     │  │ 9912    │  │700905209.82│  │84.75         │
 │2003-01-01     │  │ 9843    │  │695569156.11│  │84.84         │
 │2003-02-01     │  │ 9774    │  │688012159.54│  │84.91         │
 │2003-03-01     │  │ 9687    │  │680132848.52│  │84.81         │
 │2003-04-01     │  │ 9600    │  │673060701.90│  │84.83         │
 │2003-05-01     │  │ 9515    │  │664281393.96│  │84.81         │
 │2003-06-01     │  │ 9441    │  │657096548.33│  │84.85         │
 │2003-07-01     │  │ 9353    │  │649878820.25│  │84.89         │
 │2003-08-01     │  │ 9276    │  │645546731.59│  │84.82         │
 │2003-09-01     │  │ 9206    │  │637803071.27│  │84.82         │
 │2003-10-01     │  │ 9140    │  │632383393.41│  │84.92         │
 │2003-11-01     │  │ 9063    │  │624471206.61│  │84.84         │
 │2003-12-01     │  │ 8980    │  │618882770.52│  │84.80         │
 │2004-01-01     │  │ 8921    │  │612585010.66│  │84.86         │
 │2004-02-01     │  │ 8849    │  │607258332.29│  │84.84         │
 │2004-03-01     │  │ 8767    │  │597252971.13│  │84.80         │
 │2004-04-01     │  │ 8686    │  │593613632.25│  │84.92         │
 └Date───────────┘  └Int16────┘  └Dec(2)──────┘  └Dec(2)────────┘
── 19 rows by 4 columns ──────────────────────────────────────────
~~~

From the Loan table, we can also look at the portfolio over time by bringing in say, the balance
column, for specific AsOfDates:

~~~
      Q=LT.Query ''
      Q.GroupBy = 'OccupancyType'
      J=Q.AddColumn 'Balance0301' 'sum PaymentHistory[2004/3/1].Balance'
      J=Q.AddColumn 'Balance0401' 'sum PaymentHistory[2004/4/1].Balance'
      Q.Execute 0
────────────────────────────────────────────────────────
 ┌GroupByOccupancyType┐  ┌Balance0301─┐  ┌Balance0401─┐
 ↓Primary             │  ↓506445462.31│  ↓504070656.18│
 │Second              │  │  3785375.05│  │  3940083.96│
 │Investor            │  │ 87022133.77│  │ 85602892.11│
 │Other               │  │        0.00│  │        0.00│
 │                    │  │        0.00│  │        0.00│
 └Char(16)────────────┘  └Dec(2)──────┘  └Dec(2)──────┘
── 5 rows by 3 columns ─────────────────────────────────
~~~

In addition to specifying explicit dates, the bracket syntax allows specifying dates relative to
the maximum value of AsOfDate in the payment history table. Zero is used to represent the most
recent date, and positive integers represent the number of days earlier, while negative integer
represent the number of months earlier. This allows for reporting on the most recent activity
without changing the expressions. For example, the query above may be written as:

~~~
      Q=LT.Query ''
      Q.GroupBy='OccupancyType'
      J= Q.AddColumn 'PreviousBalance' 'sum PaymentHistory[-1].Balance'
      J=  Q.AddColumn 'CurrentBalance' 'sum PaymentHistory[0].Balance'
      Q.Execute 0
─────────────────────────────────────────────────────────────
 ┌GroupByOccupancyType┐  ┌PreviousBalance┐  ┌CurrentBalance┐
 ↓Primary             │  ↓506445462.31   │  ↓504070656.18  │
 │Second              │  │  3785375.05   │  │  3940083.96  │
 │Investor            │  │ 87022133.77   │  │ 85602892.11  │
 │Other               │  │        0.00   │  │        0.00  │
 │                    │  │        0.00   │  │        0.00  │
 └Char(16)────────────┘  └Dec(2)─────────┘  └Dec(2)────────┘
── 5 rows by 3 columns ──────────────────────────────────────
~~~

The ability to start in either table, and bring in data from the other table where appropriate,
allows us to easily solve a wide range of reporting problems, including CPR and delinquency roll rates.


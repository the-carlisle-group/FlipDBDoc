# Prepayment Rate Report

The following code creates a CPR report with two queries, demonstrating the techniques outlined in
the previous help topic. You can use the "Play" option on the File menu to execute this code in
the session. At the end, the report R may be saved using:

~~~
Scripting.Save R 'MyReport'
~~~

This will place the report in the root folder of the Server Explorer so that it may be accessed via
the GUI.

~~~
      S=Scripting.Server
      J=S.DeleteDatabase 'SampleLoan'
      D=S.BuildSampleLoan 'SampleLoan' 10000 18
      PT=D.GetTable 'PaymentHistory'
      Q1=PT.Query ''
      Q1.GroupBy='AsOfDate'
      Q1.Format='List1'
      J=Q1.AddColumn 'Count' 'count LoanNumber'
      J=Q1.AddColumn 'TotalBalance' 'sum Balance'
      J=Q1.AddColumn 'CPR1M' '"O100<n/a>F10.2" toChar 2 round 100 times 1 - (TotalBalance divide lag (sum SB1M) 1 0) power 12'
      J=Q1.AddColumn 'CPR3M' '"O100<n/a>F10.2" toChar 2 round 100 times 1 - (TotalBalance divide lag (sum SB3M) 3 0) power 4'
      J=Q1.AddColumn 'CPR6M' '"O100<n/a>F10.2" toChar 2 round 100 times 1 - (TotalBalance divide lag (sum SB6M) 6 0) power 2'
      J=Q1.AddColumn 'CPR12M' '"O100<n/a>F10.2" toChar 2 round 100 times 1 - TotalBalance divide lag (sum SB12M) 12 0'
      LT=D.GetTable 'Loan'
      Q2=LT.Query ''
      Q2.GroupBy="RateType"
      Q2.KeepEmptyGroups=0
      Q2.IncludeTotalRow=1
      Q2.Format='Summary1'
      J=Q2.AddColumn 'CurrentBalance' 'PaymentHistory[0].Balance' 0
      J=Q2.AddColumn 'Active' 'PaymentHistory[0].AsOfDate ne 0' 0
      J=Q2.AddColumn 'NumberOfLoans' 'count LoanNumber where Active'
      J=Q2.AddColumn 'TotalBalance' 'sum CurrentBalance where Active'
      J=Q2.AddColumn 'CPR1M' '2 round 100 times 1 - (TotalBalance divide sum PaymentHistory[-1].SB1M) power 12'
      J=Q2.AddColumn 'CPR3M' '2 round 100 times 1 - (TotalBalance divide sum PaymentHistory[-3].SB3M) power 4'
      J=Q2.AddColumn 'CPR6M' '2 round 100 times 1 - (TotalBalance divide sum PaymentHistory[-6].SB6M) power 2'
      J=Q2.AddColumn 'CPR12M' '2 round 100 times 1 - TotalBalance divide sum PaymentHistory[-12].SB12M'
      R=Scripting.Report.New ''
      J=R.Add Q1
      J=R.Add Q2
~~~


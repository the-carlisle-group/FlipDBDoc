# Computing Delinquency Status

Delinquency status is a critical statistic in mortgage finance. If a borrower misses one payment he
is said to be one month or 30 days delinquent. If a borrower fails to make two consecutive
payments he is said to be 2 months, or 60 days delinquent. If a borrower fails to make three
consecutive payments he is 3 months, or 90 days delinquent, and so on.

To compute the number of months delinquent we need two dates: the next payment due date for the
loan, and an "as-of" indicating where we are in time. Let's create some sample dates:

~~~
      AsOfDates = '2014/6/30,2014/6/29,2014/6/14,2014/6/13,2014/2/28'
      NextDueDates = '2014/5/1,2014/5/1,2014/5/15,2014/6/15,2013/12/31'
      AsOfDates NextDueDates
 ┌──────────┐  ┌──────────┐
 ↓2014-06-30│  ↓2014-05-01│
 │2014-06-29│  │2014-05-01│
 │2014-06-14│  │2014-05-15│
 │2014-06-13│  │2014-06-15│
 │2014-02-28│  │2013-12-31│
 └Date──────┘  └Date──────┘
~~~

Generally mortgage payments are due on the first of the month. Also generally, the "as-of" date
will be at the end of the month. This is the case for the first pair of dates in the sample data.
A simple computation of the number of months difference between these two dates produces a
possible value for months delinquent:

~~~
      MonthsDiff = AsOfDates monthsDifference NextDueDates
      AsOfDates NextDueDates MonthsDiff
 ┌──────────┐  ┌──────────┐  ┌────┐
 ↓2014-06-30│  ↓2014-05-01│  ↓1   │
 │2014-06-29│  │2014-05-01│  │1   │
 │2014-06-14│  │2014-05-15│  │1   │
 │2014-06-13│  │2014-06-15│  │0   │
 │2014-02-28│  │2013-12-31│  │2   │
 └Date──────┘  └Date──────┘  └Int8┘
~~~

This technique assumes that a payment is not late until the day the following payment is due. Thus
the May 1st payment is 1 month late on June 1st and still 1 month late on June 30th. It will not
become 2 months until July 1st. This is in fact the standard of the Office of Thrift Supervision
(OTS). However, another, slightly stricter standard is promulgated by the Mortgage Bankers
Association (MBA). Under the MBA, a payment is late if it is not made by the day BEFORE the
following payment is due. Thus a next due date of May 1st and as-of date of June 30th yields a
delinquency of 2 months, or 1 month more the OTS technique.

If the as-of dates are always precisely at month end, and payment dates are always on the first of
the month then we can use this simple formula for delinquency under the OTS standard, and then
simply add 1 month for the MBA standard:

~~~
      AsOfDates NextDueDates  (MonthsDiff + 1)
 ┌──────────┐  ┌──────────┐  ┌────┐
 ↓2014-06-30│  ↓2014-05-01│  ↓2   │
 │2014-06-29│  │2014-05-01│  │2   │
 │2014-06-14│  │2014-05-15│  │2   │
 │2014-06-13│  │2014-06-15│  │1   │
 │2014-02-28│  │2013-12-31│  │3   │
 └Date──────┘  └Date──────┘  └Int8┘
~~~

As this point, of our 5 sample pairs of dates, only the first result is correct. This expression
must now be extended to handle due dates not on the 1st of the month ("odd" due dates), in
addition to as-of dates anytime during the month. For example, in the second pair, the next due
date is still May 1st, but the as-of date is June 29th rather than June 30th, then while the OTS
delinquency is unchanged at 1, the MBA delinquency has gone from 2 to 1. Thus, in determining the
MBA value, we only want to add one month to the OTS result if the day element of the due date is
equal to the day element of the date one day after the as-of date. It is convenient to express
this condition as a boolean, where we can add the resulting 1 or 0 to our basic formula. We
will name it MBAFactor:

~~~
      MBAFactor=(day NextDueDates) eq (day AsOfDates addDays 1)
      AsOfDates NextDueDates MonthsDiff MBAFactor (MonthsDiff + MBAFactor)
 ┌──────────┐  ┌──────────┐  ┌────┐  ┌───────┐  ┌────┐
 ↓2014-06-30│  ↓2014-05-01│  ↓1   │  ↓1      │  ↓2   │
 │2014-06-29│  │2014-05-01│  │1   │  │0      │  │1   │
 │2014-06-14│  │2014-05-15│  │1   │  │1      │  │2   │
 │2014-06-13│  │2014-06-15│  │0   │  │0      │  │0   │
 │2014-02-28│  │2013-12-31│  │2   │  │0      │  │2   │
 └Date──────┘  └Date──────┘  └Int8┘  └Boolean┘  └Int8┘
~~~

A final adjustment must be made for odd due dates for both MBA and OTS. If the day element of the
as-of date is less than the day element of the next due date, then we must subtract one month from
the above expression. We can express this condition with:

~~~
      OddDayFactor = (day AsOfDates) lt (day NextDueDates)
~~~

And then look at the results:

~~~
      AsOfDates NextDueDates MonthsDiff MBAFactor OddDayFactor (MonthsDiff + MBAFactor - OddDayFactor)
 ┌──────────┐  ┌──────────┐  ┌────┐  ┌───────┐  ┌───────┐  ┌────┐
 ↓2014-06-30│  ↓2014-05-01│  ↓1   │  ↓1      │  ↓0      │  ↓2   │
 │2014-06-29│  │2014-05-01│  │1   │  │0      │  │0      │  │1   │
 │2014-06-14│  │2014-05-15│  │1   │  │1      │  │1      │  │1   │
 │2014-06-13│  │2014-06-15│  │0   │  │0      │  │1      │  │(1) │
 │2014-02-28│  │2013-12-31│  │2   │  │0      │  │1      │  │1   │
 └Date──────┘  └Date──────┘  └Int8┘  └Boolean┘  └Boolean┘  └Int8┘
~~~

However, we need to protect this expression from returning true or 1 if the as-of date is month
end - in which case its day element should not be considered less than the day element of the
next due date regardless of the actual values.

~~~
      OddDayFactor = (AsOfDates ne monthEnd AsOfDates) and (day AsOfDates) lt (day NextDueDates)
      AsOfDates NextDueDates MonthsDiff MBAFactor OddDayFactor (MonthsDiff + MBAFactor - OddDayFactor)
 ┌──────────┐  ┌──────────┐  ┌────┐  ┌───────┐  ┌───────┐  ┌────┐
 ↓2014-06-30│  ↓2014-05-01│  ↓1   │  ↓1      │  ↓0      │  ↓2   │
 │2014-06-29│  │2014-05-01│  │1   │  │0      │  │0      │  │1   │
 │2014-06-14│  │2014-05-15│  │1   │  │1      │  │1      │  │1   │
 │2014-06-13│  │2014-06-15│  │0   │  │0      │  │1      │  │(1) │
 │2014-02-28│  │2013-12-31│  │2   │  │0      │  │0      │  │2   │
 └Date──────┘  └Date──────┘  └Int8┘  └Boolean┘  └Boolean┘  └Int8┘
~~~

Finally, we need to zero out any negative results for loans that have paid in advance:

~~~
      0 high MonthsDiff + MBAFactor - OddDayFactor
┌────┐
↓2   │
│1   │
│1   │
│0   │
│2   │
└Int8┘
~~~

FlipDB wraps up this algorithm in a convenient user function →[*.delinquency]:

~~~
      delinquency AsOfDates NextDueDates
┌────┐
↓2   │
│1   │
│1   │
│0   │
│2   │
└Int8┘
~~~

The delinquency function takes an optional left argument, which defaults to 1, for the MBA
standard. Provide a 0 and it will return OTS standard:

~~~
      0 delinquency AsOfDates NextDueDates
┌────┐
↓1   │
│1   │
│0   │
│0   │
│2   │
└Int8┘
~~~


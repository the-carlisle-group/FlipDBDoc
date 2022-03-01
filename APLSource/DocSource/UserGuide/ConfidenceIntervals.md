# Confidence Intervals

The customer service department of a local gas utility would like to estimate  the average length
of time between the entry of the service request and the connection of service. A random sample of
15 houses was selected from the records available during the past year.  The results in number of
days were as follows:  114,78,96,137,78,103,117,126,86,99,114,72,104,73,86.   To find the point
estimate for the mean enter:

~~~
      DAYS=114 78 96 137 78 103 117 126 86 99 114 72 104 73 86
      mean DAYS
┌───────────────┐
│98.866666666667│
└Float──────────┘
~~~

The sample mean or point estimate is about 99 days; however, we don't know how accurate this is, so
we calculate a 95% Confidence Interval:

~~~
      95 mean confidenceInterval DAYS
┌────────────────┐
↓ 87.769564633094│
│109.96376870024 │
└Float───────────┘
~~~

We can be 95% confident that it takes between 88 and 110 days to connect gas service.  If we would
like to be more confident, we have to widen the interval:

~~~
      99 mean confidenceInterval DAYS
┌────────────────┐
↓ 83.464515968195│
│114.26881736514 │
└Float───────────┘
~~~

Estimate the proportion of gas connections that it takes more than 100 days.

~~~
      proportion DAYS > 100
┌────────────────┐
│0.46666666666667│
└Float───────────┘
      95 proportion confidenceInterval DAYS > 100
┌────────────────┐
↓0.21419948172429│
│0.71913385160904│
└Float───────────┘
~~~

We know that between 21.4% and 71.9% take that long.  This is a wide range, but if we were to
increase our sample size this range would be much smaller.

To measure the spread, we can estimate the sample standard deviation:

~~~
      standardDeviation DAYS
┌───────────────┐
│20.038771942222│
└Float──────────┘
~~~

Again, we don't know how accurate the point estimate is so we calculate a 95%  confidence interval:

~~~
      standardDeviation confidenceInterval DAYS
┌───────────────┐
↓14.670917664019│
│31.603127438393│
└Float──────────┘
~~~

This tells us that the standard deviation is somewhere between 14.7 and 31.6 days.


# Hypothesis Tests

A hypothesis test is like a jury trial where we assume the defendant is not guilty unless we have
sufficient evidence to prove otherwise. The sample data represent the evidence.  The null
hypothesis may be a manufacturer's claim or some other assumption which we assume to be true. When
the test is conducted, the p-Value represents the probability that the claim about a population is
true given the evidence (sample). If the p-Value is small, typically less than 5%, we then have
enough evidence to reject the null hypothesis. If this is not the case, we don't support the claim
or assumption, we simply say we do not have any reason to doubt it.

## Examples

### Mean:  1-sample t-test

A large chain of home improvement centers is having an end-of-season clearance of lawnmowers.  The
number of lawnmowers sold during the sale at a sample of 10 stores was as follows:
8,11,0,4,7,8,10,5,8,3.  Is there evidence that an average of more than 5 lawnmowers per store have
been sold during this sale?  We assume an average of 5 or fewer lawnmowers.  Thus the null
hypothesis is H₀:X<=5.  The alternate hypothesis is H₁:X>5 which we will use in our expression below:

~~~
      X=8 11 0 4 7 8 10 5 8 3
      X mean hypothesis > 5
┌───────────────────────────────────────────────────────┐
│ H₀:µ≤5                    H₁:µ>5                      │
│ ┌─────────────────────────┬─────────────────────────┐ │
│ │TestStatistic:           │P_Value:                 │ │
│ │                 t=1.3125│                p=0.11092│ │
│ ├─────────────────────────┼─────────────────────────┤ │
│ │CriticalValue:           │Significance:            │ │
│ │         t(⍺;df=9)=1.8331│                ⍺=0.05000│ │
│ └─────────────────────────┴─────────────────────────┘ │
│ Conclusion:  Do not reject H₀.                        │
│                                                       │
└PropertySpace──────────────────────────────────────────┘
~~~

The comparison table shown above combines the critical-value approach and the p-value approach to
hypothesis testing.  In the critical value approach we compare the test statistic obtained from
the data to a critical value obtained from  a sampling distribution. In this case the test
statistic t=(x-m)/(s/sqrt(n)) where x is the sample mean, m is the  hypothesized mean (the
assumption), s is the sample standard deviation and n is the sample size.  We compare this value
t=1.3125 to the critical value t**=1.8331 which is the critical value of the t-distribution  with
9 degrees of freedom and an upper tail of .05.  In the p-Value approach, we compare the
probability that the claim is true given the evidence (the p-Value). The p-value is 0.1109.  This
means that there is greater than a 10% chance that the sample could  have come from a population
whose mean was greater than 5. Since 11.9% is greater than 5%  we do not have enough evidence to
conclude that the average  store sold more than 5 lawnmowers at the 10% significance level. The
shaded area in the graphic below illustrates the probability that the null hypothesis is true
given the data.

~~~
                           .........              |
                         .............            |
                        ...............           |
                      ...................         |
                     .....................        |
                   .........................      |
                 ............................#    |
               ..............................###  |
            .................................#####|
        .....................................#####|####
 ............................................#####|###########
 +---------+---------+---------+---------+--------|+---------+
-3        -2        -1         0         1   t    *2         3
~~~

### 1-sample Proportion

A radio station in Myrtle Beach announced that at least 90% of the motels would be full on the
Memorial Day weekend. On Saturday a sample of 58 motels showed 49 with a no-vacancy sign and 9
with vacancies.  Test the validity of the radio station's claim.

~~~
      FULL=9 49 replicate 0 1
      FULL proportion hypothesis >= .9
┌───────────────────────────────────────────────────────┐
│ H₀:p≥0.9                  H₁:p<0.9                    │
│ ┌─────────────────────────┬─────────────────────────┐ │
│ │TestStatistic:           │P_Value:                 │ │
│ │                 Z=1.4006│               p=0.080667│ │
│ ├─────────────────────────┼─────────────────────────┤ │
│ │CriticalValue:           │Significance:            │ │
│ │              Z(⍺)=1.6449│               ⍺=0.050000│ │
│ └─────────────────────────┴─────────────────────────┘ │
│ Conclusion:  Do not reject H₀.                        │
│                                                       │
└PropertySpace──────────────────────────────────────────┘
~~~

The shaded area in the graphic below illustrates the probability that a standard normal random
variable is at least as far  away from zero in either direction as the test statistic. Note that
the test statistic Z is safely inside the critical values represented by the vertical bars.

~~~
            |              .........               |
            |            .............             |
            |           ...............            |
            |         ...................          |
            |       .......................        |
            |      .........................       |
            |    .............................     |
            |  ##.............................##   |
            |####.............................#### |
          ##|####.............................#####|#
 ###########|####.............................#####|##########
 +---------+|--------+---------+---------+---------|---------+
-3        -2*    z  -1         0         1    z    *         3
~~~

### Comparing Means:  2-sample t-test

Clearwater National Bank is conducting a study designed to identify differences between mortgage
balances from customers at two of its branch banks.  A simple random sample of 28 mortgages from
the Cherry Grove Branch and an independent sample of 22 mortgages is selected from the Beechmont
Branch.  A summary of the mortgage balances follows:

|Branch:|Cherry Grove|Beechmont|
|-|-|-|
|Sample Size|Sample Mean|Sample Standard Deviation|
|n1=28|x1=$102,500|s1=$15,000|
|n2=22|x2=$91,000|s2=$12,500|

~~~
      CG=emptyPropertySpace
      CG.count=28
      CG.mean=102500
      CG.variance=15000 power 2
      BM=emptyPropertySpace
      BM.count=22
      BM.mean=91000
      BM.variance=12500 power 2
      CG mean hypothesis eq BM
┌───────────────────────────────────────────────────────┐
│ H₀:µ₁=µ₂                       H₁:µ₁≠µ₂                     │
│ ┌─────────────────────────┬─────────────────────────┐ │
│ │TestStatistic:           │P_Value:                 │ │
│ │                 t=2.9557│               p=0.004864│ │
│ ├─────────────────────────┼─────────────────────────┤ │
│ │CriticalValue:           │Significance:            │ │
│ │      t(⍺/2;df=47)=2.0117│               ⍺=0.050000│ │
│ └─────────────────────────┴─────────────────────────┘ │
│ Conclusion:  Reject H₀.                               │
│                                                       │
└PropertySpace──────────────────────────────────────────┘
~~~

The shaded area in the graphic below illustrates the probability that a standard normal random
variable is at least as far  away from zero in either direction as the test statistic. Note that
the t-statistic is outside the critical values represented by the vertical bars; therefore we
reject Ho.

~~~
           |               .........                |
           |             .............              |
           |            ...............             |
           |          ...................           |
           |         .....................          |
           |       .........................        |
           |     .............................      |
           |   .................................    |
           | .....................................  |
         ..|........................................|.
 #.........|........................................|........#
 +---------|---------+---------+---------+---------+|--------+
-3t       -*        -1         0         1         2*        t
~~~

### Comparing Means:  Matched Samples

Eight individuals were selected at random to rate the purchase potential of a particular product
before and after seeing a television  commercial about the product.  The ratings were on a scale
from 0 to 10 with higher values indicating a higher purchase potential.  The firm wanted to test
the effectiveness of the commercial.  The following are the results of the survey:

|Individual|Before|After|
|-|-|-|
|1|2|3|4|5|6|7|8|
|5|4|7|3|5|8|5|6|
|6|6|7|4|3|9|7|6|

~~~
      BEFORE=5 4 7 3 5 8 5 6
      AFTER=6 6 7 4 3 9 7 6
      (AFTER-BEFORE) mean hypothesis <= 0
┌───────────────────────────────────────────────────────┐
│ H₀:µ≤0                    H₁:µ>0                      │
│ ┌─────────────────────────┬─────────────────────────┐ │
│ │TestStatistic:           │P_Value:                 │ │
│ │                 t=1.3572│                p=0.10842│ │
│ ├─────────────────────────┼─────────────────────────┤ │
│ │CriticalValue:           │Significance:            │ │
│ │         t(⍺;df=7)=1.8946│                ⍺=0.05000│ │
│ └─────────────────────────┴─────────────────────────┘ │
│ Conclusion:  Do not reject H₀.                        │
│                                                       │
└PropertySpace──────────────────────────────────────────┘
~~~


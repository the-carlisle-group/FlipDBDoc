# Critical Values

Instead of looking up values in a table, you can use the critical value operator  to obtain
critical values from the Normal, Student-t, ChiSquare and F Distributions.

## Normal Distribution

Find the critical value which is greater than 95% of all standard normal values.

~~~
      normal criticalValue > .95
┌───────────────┐
│1.6448534002834│
└Float──────────┘
~~~

Right tailed critical value of the Standard Normal Distribution:

~~~
      normal criticalValue < .10
┌───────────────┐
│1.2815518369953│
└Float──────────┘
~~~

Two-tailed critical value of the Standard Normal Distribution:

~~~
      normal criticalValue ne .05
┌───────────────┐
│1.9599627284195│
└Float──────────┘
~~~

A mandatory competency test for high school sophomores has a normal distribution with a mean of 400
and a standard deviation of 100.  The top 3% of students receive $500. What is the minimum score
you would need to receive the award?

~~~
      400 100 normal criticalValue < .03
┌───────────────┐
│588.07925874944│
└Float──────────┘
~~~

The bottom  1.5% of students must go to summer school. What is the minimum score you would need to
stay out of this group?

~~~
      400 100 normal criticalValue > .015
┌───────────────┐
│182.99110888231│
└Float──────────┘
~~~

## Student t Distribution

Student t 95% Confidence Level for 3 degrees of Freedom

~~~
      3 tDist criticalValue eq  .95
┌───────────────┐
│3.1824463132925│
└Float──────────┘
~~~

One-tail Student t critical values for 5 degrees of freedom:

~~~
      5 tDist criticalValue < .05 .025 .01
┌───────────────┐
↓2.0150483746518│
│2.5705818416025│
│3.3649300158603│
└Float──────────┘
~~~

Two-tail Student t critical values for 8 degrees of freedom:

~~~
      8 tDist criticalValue ne .10 .05
┌───────────────┐
↓1.8595480383807│
│2.3060041396358│
└Float──────────┘
~~~

## Chi-Square and F Distributions

Various values of the Chi-Square Distribution with 15 degrees of Freedom

~~~
      15 chiSquare criticalValue < .99 .95  .9 .1 .05 .01
┌────────────────┐
↓ 5.2293488841314│
│ 7.2609439277852│
│ 8.5467562417592│
│22.30712958198  │
│24.995790140226 │
│30.57791416721  │
└Float───────────┘
~~~

F Distribution with five degrees of freedom in numerator and three degrees of freedom in denominator:

~~~
      5 3 fDist criticalValue < .05
┌───────────────┐
│9.0134551675048│
└Float──────────┘
~~~

F Distribution with degrees of freedom reversed produces a different value:

~~~
      3 5 fDist criticalValue < .05
┌───────────────┐
│5.4094513180584│
└Float──────────┘
~~~


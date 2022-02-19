# Probability

A trial is an experiment which results in a single outcome.  An event is a set of outcomes from an
experiment.  The probability of an event is a number between 0 and 1.  A simple experiment is
flipping a coin.  There are two possible outcomes for flipping a coin:  Heads and Tails.  A more
complex experiment is flipping three coins and counting the number of heads.  To model this we use
the binomial distribution.  The number of trials is 3, and the probability of heads is 0.5.  What
is the probability that we get exactly one head?

~~~
      3 .5 binomial probability == 1
┌──────┐
│0.375 │
└Dec(3)┘
~~~

The complement is the probability of an event not happening.  What is the probability that we do
not get exactly one head?

~~~
      3 .5 binomial probability <> 1
┌──────┐
│0.625 │
└Dec(3)┘
~~~

Cumulative probability:  What is the probability that we get at most one head?

~~~
      3 .5 binomial probability <= 1
┌──────┐
│0.5   │
└Dec(1)┘
~~~

What is the probability that we get all heads or all tails?  In other words, what is the
probability that we get zero or three heads?

~~~
      3 .5 binomial probability in 0 3
┌──────┐
│0.25  │
└Dec(2)┘
~~~

Suppose a car dealer sells an average of 5 cars per day. What is the probability that he sells
exactly 3 cars on a particular day? We will model this with the Poisson distribution:

~~~
      5 poisson probability == 3
┌────────────────┐
│0.14037389581428│
└Float───────────┘
~~~

What is the probability that he sells at least 3 cars?

~~~
      5 poisson probability >= 3
┌────────────────┐
│0.87534798051692│
└Float───────────┘
~~~

What is the probability that he sells more than 3 cars?

~~~
      5 poisson probability > 3
┌────────────────┐
│0.73497408470264│
└Float───────────┘
~~~

## Continuous Distributions

We cannot compute the exact probability for a particular outcome of a continuous distribution.
However, we can calculate the probability for a range of outcomes. Probability for a continuous
distribution is defined as the area under the curve. For example, let Z be a standard normal
random variable.   Let's calculate the probability that Z is less than 1.5. The graphic below
shows the familiar bell-shaped curve of the normal distribution:

~~~

~~~

Observe that most of the area under the curve is shaded.

~~~
      normal probability < 1.5
┌───────────────┐
│0.9331927822029│
└Float──────────┘
~~~

Now let's calculate the probability that the random variable Z is greater than 1.0  Observe that
the shaded area in the graphic below is to the right of 1.0.

~~~

~~~

Less than half of the area is shaded.  In fact the area is about 16%.

~~~
      normal probability > 1
┌────────────────┐
│0.15865522781136│
└Float───────────┘
~~~

Now let's calculate the probability the Z is between 0 and 1.  The graphic below shows the area of
interest represents about one-third of the total:

~~~

~~~

We accomplish this by using the right operand "between"; the right argument contains the range of interest:

~~~
      normal probability between 0 1
┌────────────────┐
↓0.34134462228815│
└Float───────────┘
~~~

Now let's try an application.  The average height of a certain age group is 53 inches. The standard
deviation is 4 inches.  Assuming the heights are normally distributed, find the probability that a
selected individual's height will be greater than 59 inches.

~~~
      53 4 normal probability > 59
┌─────────────────┐
│0.066807217797097│
└Float────────────┘
~~~

... less than 44 inches.

~~~
      53 4 normal probability < 45
┌─────────────────┐
│0.022750058945113│
└Float────────────┘
~~~

... between 58 and 62 inches.

~~~
      53 4 normal probability between 58 62
┌─────────────────┐
│0.093425387971571│
└Float────────────┘
~~~

## Multiple Probabilities

To find multiple probabilities, enter a list of values for the right argument.

~~~
      3 .5 binomial probability == 0 1 2 3 4
┌──────┐
↓0.125 │
│0.375 │
│0.375 │
│0.125 │
│0.000 │
└Dec(3)┘
      normal probability <= -2 -1 0 1 2
┌─────────────────┐
↓0.022750058945113│
│0.15865522781136 │
│0.50000014990049 │
│0.84134477218864 │
│0.97724994105489 │
└Float────────────┘
~~~

Use a continuous distribution as a left operand with the right operand →between to obtain
probabilities for pairwise intervals.  Note that a right argument containing seven values produces
six-item result.

~~~
      normal probability between -3 -2 -1 0 1 2 3
┌─────────────────┐
↓0.021400091853164│
│0.13590516886625 │
│0.34134492208913 │
│0.34134462228815 │
│0.13590516886625 │
│0.021400091853164│
└Float────────────┘
~~~


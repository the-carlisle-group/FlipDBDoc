# Statistical Distributions

Statistical distributions can be categorized into two types:  Discrete  and Continuous.
Distribution functions can be applied to various operators to produce probabilities, goodness of
fit tests, simulations and theoretical values such as mean and standard deviation.

## Discrete Distributions

Discrete distributions take on a finite or countable infinite number of values.  Most (but not all)
discrete distributions are integer-valued and involve count data.  A discrete distribution in its
native form is the probability mass function.

|Discrete Distributions  | Parameter List|
|-|-|
|uniform               | a - lower bound (default 1), b - upper bound.|
|binomial              | n - Sample size, p - probability of success|
|poisson               | λ - average number of arrivals per time period|
|negativeBinomial      | n - number of success, p - probability of success|
|hyperGeometric        | m - number of successes , n - sample size , N - Population size|
|multinomial           | V - List of Values (default 1 thru n), P - List of probabilities totalling 1|

See →uniform, →binomial, →poisson, →negativeBinomial, →hyperGeometric, or →multinomial.

## Continuous Distributions

Continuous distributions involve measurement data such as price, length, weight, time or
temperature.  Probabilities can be measured cumulatively or on intervals, but not on individual
values.  A continuous distribution in its native form is a density function.

|Continuous Distributions                  | Parameter List|
|-|-|
|normal                                  | μ - theoretical mean (default 0); σ - standard deviation (default 1)|
|exponential                             | λ - mean time to fail|
|rectangular (continuous uniform)        | a - lower bound (default 0), b - upper bound (default 1)|
|triangular                              | a - lower bound, m - most common value, b - upper bound|
|chiSquare                               | df - degrees of freedom|
| tDist  (Student)                       | df - degrees of freedom|
| fDist                                  | df1 - degrees of freedom for numerator, df2 - degrees of freedom for denominator|
| gamma                                  | k - shape, θ - scale|
| beta                                   | ⍺, β - shape parameters|

See →normal, →exponential, →rectangular, →triangular, →chiSquare, or →tDist, →fDist, →gamma or →beta.


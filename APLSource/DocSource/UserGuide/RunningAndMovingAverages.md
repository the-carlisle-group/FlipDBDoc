# Running and Moving Averages

When time series data are available, we often want to smooth out the data.  We can use the running
operator to accomplish this.  For example, instead of looking at monthly interest rates, we may
want to look at the three-month running average over time.  Here are the 10-year treasury rates
over the last twelve months:

~~~
      TSY=2.02 1.89 1.87 1.7 2.16 2.52 2.6 2.78 2.64 2.57 2.75 3.04
~~~

To generate the three-month running averages over the period:

~~~
      enclose 2 round 3 average running TSY
┌─────────────────────────────────────────────────────────────┐
│[2.02,1.96,1.93,1.82,1.91,2.13,2.43,2.63,2.67,2.66,2.65,2.79]│
└Dec(2)───────────────────────────────────────────────────────┘
~~~

Notice that the first two terms of the result are the averages of the first and first and second
terms of the argument respectively.

To generate the three-month forward running averages over the period, use a negative left operand:

~~~
      enclose 2 round -3 average running TSY
┌─────────────────────────────────────────────────────────────┐
│[1.93,1.82,1.91,2.13,2.43,2.63,2.67,2.66,2.65,2.79,2.90,3.04]│
└Dec(2)───────────────────────────────────────────────────────┘
~~~

In this case the last two terms of the result are the averages of the last two terms and the last
term of the argument respectively.

To generate a running average over the entire period, use 0 as the left operand:

~~~
      enclose 2 round average running TSY
┌─────────────────────────────────────────────────────────────┐
│[2.02,1.96,1.93,1.87,1.93,2.03,2.11,2.19,2.24,2.28,2.32,2.38]│
└Dec(2)───────────────────────────────────────────────────────┘
~~~

We can also use the →running operator to generate cumulative monthly returns:

~~~
      CUMGROWTH=product running 1+TSY/1200
      CUMGROWTH
┌───────────────┐
↓1.0016833333333│
│1.0032609845833│
│1.0048243996176│
│1.0062479008504│
│1.008059147072 │
│1.0101760712808│
│1.0123647861019│
│1.0147100978564│
│1.0169424600717│
│1.0191204118403│
│1.0214558961175│
│1.0240435843876│
└Float──────────┘
      CUMRETURN=100 * CUMGROWTH minus 1
      enclose 2 round CUMRETURN
┌─────────────────────────────────────────────────────────────┐
│[0.17,0.33,0.48,0.62,0.81,1.02,1.24,1.47,1.69,1.91,2.15,2.40]│
└Dec(2)───────────────────────────────────────────────────────┘
~~~

We can estimate the volatility in interest rates by taking the standard deviation:

~~~
      standardDeviation TSY
┌────────────────┐
│0.43114716604412│
└Float───────────┘
~~~

We can observe how the volatility measurement stabilizes as we increase the sample size by applying
a running or cumulative standard deviation

~~~
      standardDeviation running TSY
┌─────────────────┐
↓0                │
│0.091923881554246│
│0.081445278152476│
│0.13140268896284 │
│0.17253985046939 │
│0.28675192530595 │
│0.33982488487595 │
│0.39412652065766 │
│0.39770522305402 │
│0.38902299275093 │
│0.39587417652123 │
│0.43114716604412 │
└Float────────────┘
~~~

Volatility may change over time.   We can observe this by applying the →running operator:

~~~
      3 standardDeviation running TSY
┌─────────────────┐
↓0                │
│0.091923881554246│
│0.081445278152476│
│0.10440306508911 │
│0.23259406699226 │
│0.41101500378129 │
│0.23437861108329 │
│0.13316656236958 │
│0.094516312525061│
│0.10692676621563 │
│0.090737717258764│
│0.2371356854911  │
└Float────────────┘
~~~


# Simulation

Simulation allows us to model random events.  We will use the →[*.randomVariable] operator with a
statistical distribution to accomplish this.  For example to simulate a coin toss, we use the
default parameters for the binomial distribution letting 1=heads and 0=tails.

~~~
      binomial randomVariable 1
┌────┐
↓1   │
└Int8┘
~~~

To simulate three independent coin tosses:

~~~
      binomial randomVariable 3
┌────┐
↓0   │
│1   │
│0   │
└Int8┘
~~~

We use the discrete →uniform distribution to simulate the roll of two dice:

~~~
      6 uniform randomVariable 2
┌────┐
↓1   │
│5   │
└Int8┘
~~~

## Applications

### Apartment Rentals

You own a 40-unit apartment complex.  Each month you expect between 30 and 40 units to be rented
with uniform probability. You charge $500 a month for rent. Your expenses average $15,000 a month
with a standard deviation of $3,000. What is your expected monthly profit, and what percentage of
the time do you expect to lose money?

~~~
      RENTED=30 40 uniform randomVariable 1000
      COST=15000 3000 normal randomVariable 1000
      REVENUE=500 times RENTED
      PROFIT=REVENUE-COST
      mean PROFIT
┌───────────────┐
│2525.2375114637│
└Float──────────┘
      standardDeviation PROFIT
┌───────────────┐
│3315.8751321503│
└Float──────────┘
      min PROFIT
┌────────────────┐
│-10744.702774062│
└Float───────────┘
      PROBLOSS=100 times (sum PROFIT < 0)divide count PROFIT
      PROBLOSS
┌─────┐
│22.6 │
└Float┘
~~~

### The Newsvendor Problem

You can buy newspapers wholesale for 70 cents each and sell them for a dollar. Each day you can
sell an average of 37 newspapers.  Demand follows a Poisson distribution. How many papers should
you buy to maximize your profit?

~~~
      C=.70
      P=1
      D=37 poisson randomVariable 1000
~~~

Since average demand is 37, we decide to purchase 40 newspapers just to be sure we have enough.
Our profit is expected revenue minus expenses:

~~~
      (mean P times 40 low D) - C times 40
┌─────┐
│7.716│
└Float┘
~~~

For 40 papers our expected profit is $7.72.  But we do not know that 40 is optimal.  We need to try
all possibilities within a reasonable range.  Let us try all values between 30 and 44.

~~~
      Q=30 + integers 15
      enclose Q
┌──────────────────────────────────────────────┐
│[30,31,32,33,34,35,36,37,38,39,40,41,42,43,44]│
└Int8──────────────────────────────────────────┘
      PROFIT=(mean P times Q low enclose D)- C times Q
~~~

By pairing up inventory with profit, we can see that purchasing 34 newspapers gives us the highest
profit. ($9.05)

~~~
      Q PROFIT
 ┌────┐  ┌─────┐
 ↓30  │  ↓8.713│
 │31  │  │8.871│
 │32  │  │8.987│
 │33  │  │9.049│
 │34  │  │9.053│
 │35  │  │8.997│
 │36  │  │8.868│
 │37  │  │8.674│
 │38  │  │8.407│
 │39  │  │8.079│
 │40  │  │7.699│
 │41  │  │7.267│
 │42  │  │6.789│
 │43  │  │6.27 │
 │44  │  │5.71 │
 └Int8┘  └Float┘
~~~


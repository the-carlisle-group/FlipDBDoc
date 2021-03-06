# Regression

## Introduction

The point/slope formula y = b0 + b1 ** x  defines the relationship between x and y as a straight
line.  We define the relationship in functional notation as: y=f(x).  Suppose you want to hold a
wedding reception and the cost is $100 to rent the hall plus $20 per guest. How much would it cost
to invite 25 guests?  We would first let b0 = 100; this is the fixed cost. Then we define b1 = 20;
this is the variable cost.  Now we define the relationship between the number of guests and the
total cost as f(x) = 100 + 20x.  To find the total cost, we can apply the function using simple arithmetic:

~~~
      B0=100
      B1=20
      B0 + B1 * 25
┌─────┐
│600  │
└Int16┘
~~~

Suppose we have a budget of $700.  How many guests can we invite?  This problem is slightly more
difficult.  In this case we know the answer (y=700), but we don't know the question (x=how many
guests).  We can take the equation y = 100 + 20x and solve for x using high-school algebra: x = (y-100)/20

~~~
      (700-B0)/B1
┌─────┐
│30   │
└Float┘
~~~

Now suppose that the reception hall charges $500 for 20 guests, $1000 for 50 guests and $2000 for
100 guests. In order to calculate the total cost for 40 guests, you need to determine the fixed
costs and variable costs. In the first example, we knew x (number of guests) and we had to find
y=f(x) (total cost).  In the second example, we knew y (total cost) and had to find  x (number of
guests). In this latest example y=f(x), we have to determine the relationship between x and y.  In
other words, we know both x and y and we have to find f.  Since we know f(x) is linear, this is
equivalent to finding the intercept (B0) and the slope (B1).

~~~
      X=20 50 100
      Y=500 1000 2000
      R=regress Y X
      R.Slope
┌───────────────┐
↓18.877551020408│
└Float──────────┘
      R.Intercept
┌───────────────┐
│96.938775510204│
└Float──────────┘
      2 round R.Intercept + R.Slope * 40
┌──────┐
↓852.04│
└Dec(2)┘
~~~

The fixed cost for the hall is $96.94 and the variable cost per guest is $18.18.  For 40 guests the
total cost is $852.04.

## Regression using a database and inferential statistics

Loan to Value is the ratio of the loan amount to the value of the property.  We would like examine
the relationship between loan-to-value and the interest rate. The higher the loan-to-value ratio,
the riskier the loan; therefore borrower should pay a higher interest rate.  Therefore we would
expect the coefficient of LTV to be positive. In the example below, for each 1% increase in the
loan to value we would expect an increase of 1.09 basis points in the interest rate.

~~~
      S=Scripting.Server
      LOAN=S.Get '/Databases/LoanSample/Loan/'
      L=LOAN indexRows 100 deal 10000
      RATE=L.GetColumn 'Rate'
      LTV=L.GetColumn 'OriginalLTV'
      regress RATE LTV
┌───────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ H0:ßi=0                   H1:ßi≠0                                                                     │
│                                                                                                       │
│ Coefficients:                                                                                         │
│ ───────────────────────────────────────────────────────────────────────────────────────────────────── │
│  ┌Coefficient┐  ┌Estimate─────────┐  ┌StandardError─────┐  ┌T_Ratio─────────┐  ┌P_Value────────────┐  │
│  ↓B0         │  ↓5.8036693549378  │  ↓0.45218414074517  │  ↓12.834747687908 │  ↓4.4408920985006E-16│  │
│  │B1         │  │0.018514289787601│  │0.0050728814174201│  │ 3.6496594862287│  │4.2330869346419E-4 │  │
│  └Char(2)────┘  └Float────────────┘  └Float─────────────┘  └Float───────────┘  └Float──────────────┘  │
│ ── 2 rows by 5 columns ────────────────────────────────────────────────────────────────────────────── │
│                                                                                                       │
│  AnalysisOfVariance:                                                                                  │
│ ──────────────────────────────────────────────────────────────────────────                            │
│  ┌Factor─┐  ┌SumOfSquares┐  ┌DF──┐  ┌MeanSquare┐  ┌F_Ratio┐  ┌P_Value───┐                             │
│  ↓Regress│  ↓5.578       │  ↓1   │  ↓5.5781    │  ↓13.32  │  ↓0.00042331│                             │
│  │Error  │  │41.040      │  │98  │  │0.4188    │  │0.00   │  │0.00000000│                             │
│  │Total  │  │46.618      │  │99  │  │0.0000    │  │0.00   │  │0.00000000│                             │
│  └Char(7)┘  └Dec(3)──────┘  └Int8┘  └Dec(4)────┘  └Dec(2)─┘  └Dec(8)────┘                             │
│ ── 3 rows by 6 columns ───────────────────────────────────────────────────                            │
│                                                                                                       │
│ StandardError:     0.647  R_Squared:        11.966  R_SquaredAdj:     11.067                          │
│ SampleSize:   100.000                                                                                 │
│                                                                                                       │
└PropertySpace──────────────────────────────────────────────────────────────────────────────────────────┘
~~~

## Multiple Regression

You may specify more than one dependent variable. In this case we would like to see if the original
balance of the loan and the LTV both contribute to the interest rate.

~~~
      BAL=L.GetColumn 'OriginalBalance'
      regress RATE LTV BAL
┌───────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ H0:ßi=0                   H1:ßi≠0                                                                             │
│                                                                                                               │
│ Coefficients:                                                                                                 │
│ ───────────────────────────────────────────────────────────────────────────────────────────────────────────── │
│  ┌Coefficient┐  ┌Estimate─────────────┐  ┌StandardError────────┐  ┌T_Ratio──────────┐  ┌P_Value────────────┐  │
│  ↓B0         │  ↓5.6509029632538      │  ↓0.4804562716003      │  ↓11.761534394029  │  ↓4.4408920985006E-16│  │
│  │B1         │  │0.019067174641508    │  │0.0051092833099332   │  │ 3.7318687347869 │  │3.2037362487403E-4 │  │
│  │B2         │  │0.0000015182514907882│  │0.0000016069344994716│  │ 0.94481230646765│  │3.4710222703394E-1 │  │
│  └Char(2)────┘  └Float────────────────┘  └Float────────────────┘  └Float────────────┘  └Float──────────────┘  │
│ ── 3 rows by 5 columns ────────────────────────────────────────────────────────────────────────────────────── │
│                                                                                                               │
│  AnalysisOfVariance:                                                                                          │
│ ─────────────────────────────────────────────────────────────────────────                                     │
│  ┌Factor─┐  ┌SumOfSquares┐  ┌DF──┐  ┌MeanSquare┐  ┌F_Ratio┐  ┌P_Value──┐                                      │
│  ↓Regress│  ↓5.952       │  ↓2   │  ↓2.9762    │  ↓7.099  │  ↓0.0013265│                                      │
│  │Error  │  │40.666      │  │97  │  │0.4192    │  │0.000  │  │0.0000000│                                      │
│  │Total  │  │46.618      │  │99  │  │0.0000    │  │0.000  │  │0.0000000│                                      │
│  └Char(7)┘  └Dec(3)──────┘  └Int8┘  └Dec(4)────┘  └Dec(3)─┘  └Dec(7)───┘                                      │
│ ── 3 rows by 6 columns ──────────────────────────────────────────────────                                     │
│                                                                                                               │
│ StandardError:     0.647  R_Squared:        12.768  R_SquaredAdj:     10.970                                  │
│ SampleSize:   100.000                                                                                         │
│                                                                                                               │
└PropertySpace──────────────────────────────────────────────────────────────────────────────────────────────────┘
~~~


# Analysis of Variance (ANOVA)

## One-Way ANOVA

We use one-way analysis of variance to test whether the differences of three or more means are different.

~~~
      S=Scripting.Server
      LOAN=S.Get '/Databases/LoanSample/Loan/'
      L=LOAN indexRows 30 deal 10000
      RATE=L.GetColumn 'Rate'
      PT=L.GetColumn 'PropertyType'
      anova RATE PT
┌──────────────────────────────────────────────────────────────────────────────┐
│ H₀:µ₁=µ₂=µ₃               H₁:(∃ i≠j) µi≠µj                                   │
│                                                                              │
│ AnalysisOfVariance:                                                          │
│ ────────────────────────────────────────────────────────────────────────     │
│  ┌Factor─┐  ┌SumOfSquares┐  ┌DF──┐  ┌MeanSquare┐  ┌F_Ratio┐  ┌P_Value─┐      │
│  ↓A      │  ↓1.1514      │  ↓2   │  ↓0.57571   │  ↓3.1183 │  ↓0.060475│      │
│  │Error  │  │4.9848      │  │27  │  │0.18462   │  │0.0000 │  │0.000000│      │
│  │Total  │  │6.1362      │  │29  │  │0.00000   │  │0.0000 │  │0.000000│      │
│  └Char(5)┘  └Dec(4)──────┘  └Int8┘  └Dec(5)────┘  └Dec(4)─┘  └Dec(6)──┘      │
│ ── 3 rows by 6 columns ─────────────────────────────────────────────────     │
│                                                                              │
│ StandardError:     0.430  R_Squared:        18.764  R_SquaredAdj:     12.747 │
│ SampleSize:    30.000                                                        │
│                                                                              │
└PropertySpace─────────────────────────────────────────────────────────────────┘
~~~

## Two-Way ANOVA

We use two-way analysis of variance to test the main effects of each factor and their interaction.

~~~
      OT=L.GetColumn 'OccupancyType'
      anova RATE PT OT
┌──────────────────────────────────────────────────────────────────────────────┐
│                                                                              │
│ AnalysisOfVariance:                                                          │
│ ─────────────────────────────────────────────────────────────────────────    │
│  ┌Factor─┐  ┌SumOfSquares┐  ┌DF──┐  ┌MeanSquare┐  ┌F_Ratio┐  ┌P_Value──┐     │
│  ↓A      │  ↓1.1514      │  ↓2   │  ↓0.57571   │  ↓9.215  │  ↓0.0013401│     │
│  │B      │  │0.9345      │  │2   │  │0.46726   │  │7.479  │  │0.0035272│     │
│  │AB     │  │2.7383      │  │4   │  │0.68456   │  │10.957 │  │0.0000586│     │
│  │Error  │  │1.3120      │  │21  │  │0.06248   │  │0.000  │  │0.0000000│     │
│  │Total  │  │6.1362      │  │29  │  │0.00000   │  │0.000  │  │0.0000000│     │
│  └Char(5)┘  └Dec(4)──────┘  └Int8┘  └Dec(5)────┘  └Dec(3)─┘  └Dec(7)───┘     │
│ ── 5 rows by 6 columns ──────────────────────────────────────────────────    │
│                                                                              │
│ StandardError:     0.250  R_Squared:        78.619  R_SquaredAdj:     70.474 │
│ SampleSize:    30.000                                                        │
│                                                                              │
└PropertySpace─────────────────────────────────────────────────────────────────┘
~~~


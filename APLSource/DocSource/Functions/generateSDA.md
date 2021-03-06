# generateSDA

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Financial{.info}

Generate "Standard Default Assumption" curve.{.purpose}

~~~
R=[Y] generateSDA X
~~~

X is a percentage value. Y is optional loan age.
R is the age-adjusted X percent of SDA.

## Examples
~~~
      generateSDA 100
┌─────┐
↓0.02 │
│0.04 │
│0.06 │
│0.08 │
│0.1  │
│0.12 │
│0.14 │
│0.16 │
│0.18 │
│...  │
└Float┘

      24 generateSDA 100
┌─────┐
↓0.5  │
│0.52 │
│0.54 │
│0.56 │
│0.58 │
│0.6  │
│0.6  │
│0.6  │
│0.6  │
│ ... │
└Float┘

      0 18 36 generateSDA 100 200 300
┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
↓[0.02,0.04,0.06,0.08,0.1,0.12,0.14,0.16,0.18,0.2,0.22,0.24,0.26,0.28,0.3,0.32,0.34,0.36,0.38,0.4,0.42,0.44,0.46,0.48,0.5,...]│
│[0.76,0.8,0.84,0.88,0.92,0.96,1,1.04,1.08,1.12,1.16,1.2,1.2,1.2,1.2,1.2,1.2,1.2,1.2,1.2,1.2,1.2,1.2,1.2,1.2,...]             │
│[1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.8,1.7715,...]                 │
└Float────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
~~~

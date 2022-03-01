# percentDown

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Arithmetic{.info}

Computes percentage of each group relative to all groups.{.purpose}

~~~
R=[Y] percentDown X [B]
~~~

X may be numeric or Char. B is an optional Boolean restriction of X. Y is integer. R is the
percentage of X where B relative to the grand total of X. If X is character, the percentage is
based on row count. R is Float if Y is not provided. R is rounded to Integer if Y is 0, else R is
Decimal and rounded to Y decimal places.

This function has special handling when used in the context of a cross-tab query.

B is generally omitted when percentDown is used under a cross-tab query.

See also: →[percentAcross], →[percentOverall]

For numeric X, when B is not provided, this function is equivalent to:

~~~
Y round 100 * (sum X) / sum sum X
~~~

For numeric X, when B is provided, this function is equivalent to:

~~~
Y round 100 * (sum X where B) / sum sum X
~~~

For char X, when B is not provided, this function is equivalent to:

~~~
Y round 100 * (count X) / sum count X
~~~

For char X, when B is provided, this function is equivalent to:

~~~
Y round 100 * (count X where B) / sum count X
~~~

## Examples

~~~
      a=enclose (100 100) (300) (200 50 250)
      b=enclose (1 0) (1) (1 0 1)
      a b
 ┌────────────┐  ┌───────┐
 ↓[100,100]   │  ↓[1,0]  │
 │[300]       │  │[1]    │
 │[200,50,250]│  │[1,0,1]│
 └Int16───────┘  └Boolean┘
      percentDown a
┌─────┐
↓20   │
│30   │
│50   │
└Float┘
      percentDown a b
┌─────┐
↓10   │
│30   │
│45   │
└Float┘
      percentDown a (not b)
┌─────┐
↓10   │
│ 0   │
│ 5   │
└Float┘
~~~


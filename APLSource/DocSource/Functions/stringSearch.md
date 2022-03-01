# stringSearch

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational,Text{.info}

Search for strings in a char column.{.purpose}

~~~
R=X stringSearch Y
~~~

X and Y must be Char. Y is a scalar or simple. R is boolean.

R will be 1 where rows of X contain one or more of the strings in Y as a substring. The search is
case-insensitive. Commas required in literal arguments will require escaping as: "this, string"

## Examples

~~~
      (M) (M stringSearch 'uar,emb') (M stringSearch 'R')
 ┌─────────┐  ┌───────┐  ┌───────┐
 ↓January  │  ↓1      │  ↓1      │
 │February │  │1      │  │1      │
 │March    │  │0      │  │1      │
 │April    │  │0      │  │1      │
 │May      │  │0      │  │0      │
 │June     │  │0      │  │0      │
 │July     │  │0      │  │0      │
 │August   │  │0      │  │0      │
 │September│  │1      │  │1      │
 │October  │  │0      │  │1      │
 │November │  │1      │  │1      │
 │December │  │1      │  │1      │
 └Char(9)──┘  └Boolean┘  └Boolean┘
~~~

For more complex searches see the family of regex functions →[regexSearch], etc.


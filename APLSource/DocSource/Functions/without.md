# without

Type:{.prefix}

{.info}

Categories:{.prefix}

{.info}

Selection{.purpose}

~~~
R=X without Y
~~~

X and Y must be compatible types. R has type and value of X with elements that also occur anywhere
in Y removed. The structure of R is also that of X excepting that Scalar X is converted to a one
or zero item Simple.

## Examples

~~~
      a b c d e
 ┌───────┐  ┌────┐  ┌────┐  ┌───────────┐  ┌───────────┐
 │0      │  ↓0   │  ↓2   │  │[3,4,5,6,7]│  ↓[4,5,6,7]  │
 └Boolean┘  │1   │  │3   │  └Int8───────┘  │[5,6,7,8,9]│
            │2   │  │4   │                 └Int8───────┘
            └Int8┘  │5   │
                    └Int8┘
      a without each a b c d e
 ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐
 ↓Boolean┘  ↓Boolean┘  ↓0      │  ↓0      │  ↓0      │
                       └Boolean┘  └Boolean┘  └Boolean┘
      b without each a b c d e
 ┌────┐  ┌────┐  ┌────┐  ┌────┐  ┌────┐
 ↓1   │  ↓Int8┘  ↓0   │  ↓0   │  ↓0   │
 │2   │          │1   │  │1   │  │1   │
 └Int8┘          └Int8┘  │2   │  │2   │
                         └Int8┘  └Int8┘
      c without each a b c d e
 ┌────┐  ┌────┐  ┌────┐  ┌────┐  ┌────┐
 ↓2   │  ↓3   │  ↓Int8┘  ↓2   │  ↓2   │
 │3   │  │4   │          └Int8┘  │3   │
 │4   │  │5   │                  └Int8┘
 │5   │  └Int8┘
 └Int8┘
      d without each a b c d e
 ┌───────────┐  ┌───────────┐  ┌─────┐  ┌────┐  ┌────┐
 │[3,4,5,6,7]│  │[3,4,5,6,7]│  │[6,7]│  │[]  │  │[3] │
 └Int8───────┘  └Int8───────┘  └Int8─┘  └Int8┘  └Int8┘
      e without each a b c d e
 ┌───────────┐  ┌───────────┐  ┌─────────┐  ┌─────┐  ┌────┐
 ↓[4,5,6,7]  │  ↓[4,5,6,7]  │  ↓[6,7]    │  ↓[]   │  ↓[]  │
 │[5,6,7,8,9]│  │[5,6,7,8,9]│  │[6,7,8,9]│  │[8,9]│  │[]  │
 └Int8───────┘  └Int8───────┘  └Int8─────┘  └Int8─┘  └Int8┘
~~~


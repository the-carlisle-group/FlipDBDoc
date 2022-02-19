# inChar

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Relational,Text{.info}

Mark the existence of specific characters in a string.{.purpose}

~~~
R=X inChar Y
~~~

X is a scalar or simple char column. Y is scalar char, interpreted as a list of characters. R is an
enclosed or partitioned Boolean, marking the location of characters in X that exist in Y.

Because inChar operates at the character level, the result is partitioned or enclosed. It is often
useful to apply a summary function like →[sum] or →[any] to the result.

Note that inChar will not mark trailing blanks in a string.

## Examples:

~~~
      a='Paul,Phil,Samuel,Stella'
      a
┌───────┐
↓Paul   │
│Phil   │
│Samuel │
│Stella │
└Char(6)┘
      a inChar 'l'
┌─────────────┐
↓[0,0,0,1]    │
│[0,0,0,1]    │
│[0,0,0,0,0,1]│
│[0,0,0,1,1,0]│
└Boolean──────┘
      sum a inChar 'l'
┌────┐
↓1   │
│1   │
│1   │
│2   │
└Int8┘
~~~


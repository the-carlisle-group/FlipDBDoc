# occurrences

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Count the occurrences of each item in a column{.purpose}

~~~
R=occurences X
~~~

X can be any column but Blob. R is integer being the number of times
each item in X occurs in X.

## Examples

~~~
 occurrences 'A,B,C,A,C,A'
┌────┐
↓3   │
│1   │
│2   │
│3   │
│2   │
│3   │
└Int8┘
      occurances 1 2 3 4
┌────┐
↓1   │
│1   │
│1   │
│1   │
└Int8┘
~~~


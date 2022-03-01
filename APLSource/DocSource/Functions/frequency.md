# frequency

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Statistical{.info}

~~~
R=[Y] frequency X
~~~

X is any column. Y is an optional list of values.
If Y is omitted, R is the frequency of the unique items in X,
returned in the order of appearance in X.
If Y is provided, R is corresponding frequency of items in X.

## Examples:
~~~
      A='NY,NY,NJ,CT,CT,NJ,CT,NY,NY'
      frequency A
┌────┐
↓4   │
│2   │
│3   │
└Int8┘
      uniqueValues A
┌───────┐
↓NY     │
│NJ     │
│CT     │
└Char(2)┘

      'CT,MA,NY'  frequency A
┌────┐
↓3   │
│0   │
│4   │
└Int8┘

      'NY' frequency A
┌────┐
│4   │
└Int8┘

      sum A in 'NY'
┌────┐
│4   │
└Int8┘
~~~

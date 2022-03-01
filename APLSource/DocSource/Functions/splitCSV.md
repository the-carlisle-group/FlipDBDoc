# splitSCV

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Text{.info}

Split a string into multiple strings.{.purpose}

~~~
R=[X] splitCSV Y
~~~

X is single character that defaults to comma. Y is a scalar or simple char column. R is an enclosed
or partitioned char column, with the items in Y split into multiple items based on the delimiter X.

This function is often used with the →[first], →[last] and →[nth] functions to extract words from a string.

See also →[split], which accepts multiple delimiters and treats consecutive delimiters as one.

## Examples:

~~~
      a='Edgar Allan Poe,Adam Smith,Socrates,Chris J. Date'
      a
┌───────────────┐
↓Edgar Allan Poe│
│Adam Smith     │
│Socrates       │
│Chris J. Date  │
└Char(15)───────┘
      split a
┌─────────────────┐
↓[Edgar,Allan,Poe]│
│[Adam,Smith]     │
│[Socrates]       │
│[Chris,J.,Date]  │
└Char(8)──────────┘
      last split a
┌────────┐
↓Poe     │
│Smith   │
│Socrates│
│Date    │
└Char(8)─┘
~~~


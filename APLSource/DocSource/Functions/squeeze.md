# squeeze

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Removes extraneous blanks or other specified character from a string.{.purpose}

~~~
R=[X] squeeze Y
~~~

X is a scalar Char, and defaults to blank. Y is Char. R is Y with the extraneous character defined
in X removed. If X is a blank then all leading, trailing, and embedded extraneous blanks are removed.

## Examples

~~~
      a= '   Adam  Smith    '
      a
┌──────────────┐
│   Adam  Smith│
└Char(18)──────┘
      squeeze a
┌──────────┐
│Adam Smith│
└Char(11)──┘
      squeeze 'Mississippi'
┌───────────┐
│Mississippi│
└Char(11)───┘
      's' squeeze 'Mississippi'
┌─────────┐
│Misisippi│
└Char(9)──┘
      'p' squeeze 'Mississippi'
┌──────────┐
│Mississipi│
└Char(10)──┘
~~~


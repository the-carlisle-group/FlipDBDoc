# subStringIndex

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Find the location of a substring in a string.{.purpose}

~~~
R=X subStringIndex Y
~~~

X is char. Y is a scalar char, and represents a substring. R is integer, and is the location of the
beginning of the first occurrence of the substring Y found in X. If the substring is not found, the
result is equal to the length of the string in X.

## Examples

~~~
      a='January,February,March'
      a
┌────────┐
↓January │
│February│
│March   │
└Char(8)─┘
      a subStringIndex 'uary'
┌────┐
↓3   │
│4   │
│5   │
└Int8┘
      length a
┌────┐
↓7   │
│8   │
│5   │
└Int8┘
~~~


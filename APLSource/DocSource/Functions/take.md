# take

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Selection{.info}

Select leading or trailing portion of a column or an object array.{.purpose}

~~~
R=X take Y
~~~

X is a scalar Integer. Y is any column or an object array.

R is X elements, partitions or objects from the start of Y if X is positive or the end if X is negative.

If absoluteValue X (see →[absoluteValue]) is greater than shape Y (see →[shape])  and Y is a column the result is padded with
extra items, at the end if X is positive, at the start if negative, each being equivalent to the
first item but with characters replaced by blanks or numbers by zeros; except that
absoluteValue X may not be greater than the length of an object array.

shape R is absoluteValue X

## Examples:

~~~
      3 take 1 2 3 4 5 6 7
┌────┐
↓1   │
│2   │
│3   │
└Int8┘
      -2 take 'Bob,Joe,Steve'
┌───────┐
↓Joe    │
│Steve  │
└Char(5)┘
~~~


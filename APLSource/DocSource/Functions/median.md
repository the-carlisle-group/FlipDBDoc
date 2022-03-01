# median

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

Calculate the middle value or the average of the two middle values of Field X{.purpose}

~~~
R=median X
~~~

X must be a numeric field.

R is the middle value of a set of numbers. Class is the same where ever possible.  For an even
number of items, the number of decimal places may increase by one.

## Examples

Same data using partitioned input.

~~~
      median 2 4 8
┌────┐
│4   │
└Int8┘
~~~

Integer input produces Decimal(1) output.

~~~
      median 1 2 5 9
┌──────┐
│3.5   │
└Dec(1)┘
~~~

Decimal(2) produces Decimal(2).

~~~
      median 2.45 3.67 8.45 9.23
┌──────┐
│6.06  │
└Dec(2)┘
~~~

Decimal(2) produces Decimal(3)

~~~
      median 2.45 3.67 8.46 9.23
┌──────┐
│6.065 │
└Dec(3)┘
~~~

Float always produces float.

~~~
      median 1/2 3 4 5
┌────────────┐
│0.2916666667│
└Float───────┘
~~~

~~~
      median enclose (2 4 8)(1 2 5 9)
┌──────┐
↓4.0   │
│3.5   │
└Dec(1)┘
~~~


# absoluteValue

## Type

Scalar

## Categories

Arithmetic

## Purpose

absolute, unsigned

## Syntax

~~~
R=absoluteValue X
~~~

X must be numeric. R is the absolute value of X.  R is numeric.

## Examples

~~~
      absoluteValue -3
┌────┐
│3   │
└Int8┘
      absoluteValue 5 0 -3
┌────┐
↓5   │
│0   │
│3   │
└Int8┘
~~~


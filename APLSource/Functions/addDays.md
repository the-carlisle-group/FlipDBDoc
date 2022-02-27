# addDays

## Type

Scalar

## Categories

DateTime

## Purpose

Add days to a date.

## Syntax

~~~
R=X addDays Y
~~~

X must be Date or DateTime, Y must be Integer. R has type of X with Y days added to X. If Y is
negative R is earlier than X. If X is zero (null), R is zero.

## Notes

See also: [addMonths]() [addYears]()

## Examples

~~~
      D1='2004-02-29'
      D1 addDays 366
┌──────────┐
│2005-03-01│
└Date──────┘
      D1 addDays -2
┌──────────┐
│ 2004-2-27│
└Date──────┘
~~~


# cobolComp3

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=[X[E]] cobolComp3 Y
~~~

Y must be Char and is interpreted as a COBOL COMP-3 field. X represents the implied number of
decimal places in Y. X is optional and may be a 0, the default, or an Integer from 1 to 9. If X is
omitted or 0, R is Y converted to Integer. If X is greater than 0, then R is converted to Dec(X).

E is the expected encoding of Y and may be EBCDIC (the default) or ASCII. In most cases the
encoding will be EBCDIC, and it is only necessary to specify ASCII if the file has previously
been converted to ASCII. If E is provided, X must be provided as well.

This function is normally only used when importing COBOL files. In COBOL terminology, COMP-3 is
short for COMPUTATIONAL-3, and is also sometimes referred to as Packed or Packed Decimal.

## Examples

~~~
      '00_,2#?'
┌───────┐
↓00_    │
│2#?    │
└Char(3)┘
      cobolComp3'00_,2#?'
┌──────┐
↓30,305│
│32,233│
└Int16─┘
      5 cobolComp3 '00_,2#?'
┌───────┐
↓0.30305│
│0.32233│
└Dec(5)─┘
      a=cobolChar '00_,2#?'
      2 'ASCII' cobolComp3 a
┌──────┐
↓303.05│
│322.33│
└Dec(2)┘
~~~


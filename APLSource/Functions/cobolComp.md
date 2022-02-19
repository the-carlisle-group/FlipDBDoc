# cobolComp

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=[X[E]] cobolComp Y
~~~

Y must be Char and is interpreted as a COBOL COMP field. X represents the implied number of decimal
places in Y. X is optional and may be a 0 (the default) or an Integer from 1 to 9. If X is
omitted or 0, R is Y converted to Integer. If X is greater than 0, then R is converted to Dec(X).

E is the expected encoding of Y and may be EBCDIC (the default) or ASCII. In most cases the
encoding will be EBCDIC, and it is only necessary to specify ASCII if the file has previously
been converted to ASCII. If E is provided, X must be provided as well.

This function is normally only used when importing COBOL files. In COBOL terminology, COMP is short
for COMPUTATIONAL, and is also sometimes referred to as BINARY.

## Examples

~~~
      hexToChar 'FFFF2BAC'
┌───────┐
│ÿÿ+¬   │
└Char(4)┘
      cobolComp hexToChar 'FFFF2BAC'
┌────────┐
│(54,356)│
└Int32───┘
      a=cobolChar hexToChar 'FFFF2BAC'
      a
┌───────┐
│ÿÿÔ    │
└Char(4)┘
      4 'ASCII' cobolComp a
┌────────┐
│(5.4356)│
└Dec(4)──┘
~~~


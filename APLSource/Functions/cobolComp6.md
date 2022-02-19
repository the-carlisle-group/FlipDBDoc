# cobolComp6

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=[X[E]] cobolComp6 Y
~~~

Y must be Char and is interpreted as a COBOL COMP-6 field. X represents the implied number of
decimal places in Y. X is optional and may be a 0, the default, or an Integer from 1 to 9. If X is
omitted or 0, R is Y converted to Integer. If X is greater than 0, then R is converted to Dec(X).

E is the expected encoding of Y and may be EBCDIC (the default) or ASCII. In most cases the
encoding will be EBCDIC, and it is only necessary to specify ASCII if the file has previously
been converted to ASCII. If E is provided, X must be provided as well.

This function is normally only used when importing COBOL files. In COBOL terminology, COMP-6 is
short for COMPUTATIONAL-6, and is also sometimes referred to as an unsigned packed or unsigned
packed decimal field. It is similar to COMP-3, except that while COMP-3 is signed, COMP-6 is
unsigned. COMP-3 is a much more commonly occurring field type than COMP-6.

~~~
      hexToChar  '271828'
┌───────┐
│'(     │
└Char(3)┘
      cobolComp6 hexToChar  '271828'
┌───────┐
│271,828│
└Int32──┘
      5 cobolComp6 hexToChar  '271828'
┌───────┐
│2.71828│
└Dec(5)─┘
      a=cobolChar hexToChar  '271828'
      5 'ASCII' cobolComp6 a
┌───────┐
│2.71828│
└Dec(5)─┘
~~~


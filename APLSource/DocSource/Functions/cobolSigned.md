# cobolSigned

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=[X[E]] cobolSigned Y
~~~

Y must be Char and is interpreted as a COBOL signed field. X represents the implied number of
decimal places in Y. X is optional and may be a 0, the default, or an Integer from 1 to 9. If X is
omitted or 0, R is Y converted to Integer. If X is greater than 0, then R is converted to Dec(X).

E is the expected encoding of Y and may be EBCDIC (the default) or ASCII. In most cases the
encoding will be EBCDIC, and it is only necessary to specify ASCII if the file has previously
been converted to ASCII. If E is provided, X must be provided as well.

This function is normally only used when importing COBOL files. In COBOL terminology, signed is
also sometimes referred to as zoned or zoned decimal.

## Examples

~~~
    hexToChar 'F0F0F1F2D3'
┌───────┐
│ððñòÓ  │
└Char(5)┘
    cobolChar hexToChar 'F0F0F1F2D3'
┌───────┐
│0012L  │
└Char(5)┘
   cobolSigned hexToChar 'F0F0F1F2D3'
┌─────┐
│(123)│
└Int8─┘
   0 'ASCII' cobolSigned '0012L'
┌─────┐
│(123)│
└Int8─┘
   2 'ASCII' cobolSigned '0012L'
┌──────┐
│(1.23)│
└Dec(2)┘
~~~


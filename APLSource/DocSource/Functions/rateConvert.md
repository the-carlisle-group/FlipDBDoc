# rateConvert

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Convert between nominal and/or effective interest rates{.purpose}

~~~
R=F1 F2 rateConvert I
~~~

* I is the interest rate or yield as a percentage.
* F1 is the input compounding frequency. Integer
* F2 is the output compounding frequency. Integer

R is the converted interest rate. R is Float. F1 is the compounding frequency of I. F2 is the
compounding frequency of R. Use 0 for continuous compounding.

## Examples

Convert mortgage rate to bond equivalent yield (BEY):

~~~
      12 2 rateConvert 6.5
┌───────────────┐
│6.5886591275081│
└Float──────────┘
~~~

Convert mortgage rate to effective annual rate:

~~~
      12 1 rateConvert 6.5
┌───────────────┐
│6.6971852002543│
└Float──────────┘
~~~

Convert effective rates to continuous rates:

~~~
      1 0 rateConvert 6 6.5 7
┌───────────────┐
↓5.8268908123976│
│6.2974799161388│
│6.7658648473815│
└Float──────────┘
~~~


# mortgageRollBalance

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Roll the balance of a mortgage forward or backward a specified number of months.{.purpose}

~~~
R=mortgageRollBalance B I P DR|M
~~~

* B is the current balance of the mortgage. Numeric.
* I is the interest rate expressed as a percentage. Numeric.
* P is the monthly payment. Numeric.
* DR is the date range;     DR = PD RD [DC]
* PD is the pay-thru date (Start Date). Date.
* RD is the roll-to date (End Date). Date.
* DC is the day count method.  0 or Omitted = US (NASD) 30/360; 1=Actual/Actual; 2=Actual/360; 3=Actual/365. Integer.
* M is the number of months to roll forward/backward (assumes 30/360 day count).

R is the balance of the mortgage at a particular point in time, rounded to two decimal places. This
is the future value if the roll-to date is later than the pay-thru date. If the roll-to date is
earlier than the pay-thru date then this is a prior balance.

All inputs except M (months to roll) must be positive. Unexpected values may occur with negative numbers.

The input for Daycount may only consist of the integers 0-3.

See also: →[mortgagePayment], →[mortgageRate], →[mortgageTerm]

## Examples

~~~
      mortgageRollBalance 16500000 8.42 125936.42 '1997/07/01' '1997/08/01' (0 1 2 3)
┌─────────────┐
↓16,489,838.58│
│16,492,058.92│
│16,493,697.75│
│16,492,058.92│
└Dec(2)───────┘
~~~

Roll forward 3 months:

~~~
      mortgageRollBalance 16500000 8.42 125936.42 3
┌─────────────┐
│16,469,301.34│
└Dec(2)───────┘
~~~

Roll backward 3 months:

~~~
      mortgageRollBalance 16500000 8.42 125936.42 -3
┌─────────────┐
│16,530,061.41│
└Dec(2)───────┘
      mortgageRollBalance 16500000 8.42 125936.42 '1997/07/01' '1997/04/01' (0 1 2 3)
┌─────────────┐
↓16,530,061.41│
│16,527,214.95│
│16,522,414.60│
│16,527,214.95│
└Dec(2)───────┘
~~~


# smmToCPR

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Convert SMM (single monthly mortality) to CPR (constant prepayment rate){.purpose}

~~~
R=smmToCPR Y
~~~

Y is a SMM value, R is CPR.

This function is equivalent to:

~~~
   R = 100 times 1 - (1 - Y) power 12
~~~

## Examples
~~~
      A = .01 .002 .003
      smmToCPR A
┌────────────────┐
↓11.361512828387 │
│ 2.3737752105285│
│ 3.5411900096784│
└Float───────────┘

      100 times 1 - (1 -A) power 12
┌────────────────┐
↓11.361512828387 │
│ 2.3737752105285│
│ 3.5411900096784│
└Float───────────┘
 ~~~

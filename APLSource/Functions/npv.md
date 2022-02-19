# npv

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Financial{.info}

Net PresentValue{.purpose}

~~~
R=X npv Y
~~~

* X is the discount rate in decimal form.
* Y is a series of cash flows.

R is the net present value. Net present Value is the total present value of all cash flows at the
discount rate specified in the left argument. A positive net present value is a good investment, a
negative net present value is a bad investment.

See also: →[irr]

## Examples

~~~
      .05 npv -100 50 50 25
┌───────────────┐
│13.872820481178│
└Float──────────┘
~~~


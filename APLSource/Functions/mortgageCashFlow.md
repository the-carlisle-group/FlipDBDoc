

# mortgageCashFlow

Type:{.prefix}

Misc{.info}

Categories:{.prefix}

Financial{.info}

Generate mortgage cash flows and compute prices and yields.{.purpose}

See also →[mortgageCashFlowYield], →[mortgageCashFlowPrice]
and →[mortgageCashFlowPresentValue]. 

~~~
R=mortgageCashFlow X
~~~

X is a property space containing appropriate inputs specified below.
R is a property space containing detailed and aggregated cash flows, prices and yields specified below.

This is a complex function with many inputs.
Please see the →[*.MortgageCashFlows|Mortgage Cash Flows] section of the user guide
for a discusssion and examples.

## Inputs

The right argument X is a property space that may contain zero or more of the
following named inputs. Every input has a default value, and any input may be elided.
Each input may be scalar or simple (a list) for pricing one or more mortgages.
Scalar extension applies, so providing say 1 balance but 500 rates will return
500 cash flows, yields, prices, etc.

This function may be used in ungrouped or grouped queries,
providing loan level or repline pricing respectively.
If used in a grouped query, then inputs must be aggregrated.

1. **ID**

 A loan number, rep line description, or other identifier. Must be character.
The default is '00000001',  '00000002', '000000003', etc.

1.  **StatedPrice**

 The price used to compute the ComputedYield output. The default is 0.

1. **StatedYield**

 The yield used to compute the ComputedPrice output. The default is 0.

1. **Balance**

 The balance of the loan. This may be the original balance on a new loan, or the current balance on a seasoned loan.
Numeric. Default value is 100,000.
It is always the total balance of the loan, including any nonperfoming component.

1. **NonperformingBalance**

 The nonperforming balance of the loan. Defaults to 0.
Balance less NonperformingBalance determines the performing balance.

1. **Rate**

 The rate for the life of the loan for a fixed rate loan,
or the rate for the first rate adjustment period for an adjustable loan.
Numeric. If adjustable, and the Rate is 0, then rate for the first
rate adjustment period (as all subsequent periods)
is determined by Index, Margin, etc.
The default value is 6.

1. **Term**

 The term of the loan.
This may be the original term on a new loan or the remaining term on a seasoned loan.
Integer. Defaults to 360. Must be integer greater than 0.

1. **Payment**

 The monthly principal and interest payment.
If non-zero this is used to amortize the loan.
If zero, the payment is calculated by fully amortizing the loan according to the AmortizationTerm, Balance, and Rate.
For a fixed rate loan Payment and Amorization are mutually exclusive - only one is used to generate the cashflow, and payment
takes precedence. For adjustable rate loans, if non-zero, this is the first or current payment. The AmortizationTerm
will be used to determine subsequent payments at payment change periods.

1. **AmortizationTerm**

 The term used to amortize the loan when the Payment is 0. Integer, defaults to 360.
If Payment and AmortizationTerm are both 0, then the loan is considered interest only.

1. **IOPeriod**

 The interest only period on the loan.
This may be the original IO period on a new loan,
or the remaining IO period on a seasoned loan. Integer.
The default value is 0.

1. **InactivityPeriod**

 The inactivity period shifts the cashflows out by the specified number of months.
This may be useful for loans in forbearance or loans being modified.
The inactivity period is applied at very end of the cashflow generation process.
Once the cashflows have been generated according to all of the other parameters,
all the flows are shifted out by padding with zeros with the exeption of
BeginningPerformingBalance, BeginningNonperformingBalance, PerformingBalance'
NonperformingBalance, Balance, and DelinquentBalance, which are shifted
out by replicating the first element of each flow.
The default value is 0.

1. **ServiceFee**

 The service fee on the loan. Numeric.
Provided as a percentage, for example 0.25.  The default is 0.

1. **PenaltyFee**

 Prepayment penalty fee. A percentage, for example   2.5.  Default is 0.

1. **PenaltyPeriod**

 Prepayment penalty period. The number of month for which the prepayment penalty rate it in effect.

1. **StepLoan**

 This is a boolean flag indicating that the loan is a step-rate loan.
The default is 0. If set to 1, then StepPeriods and StepIncrements are
used to determine the interest rates over time.

1. **StepPeriods**

 Periods corresponding to StepIncrements. For example, 6 12 24 means the rate will
step up in month 6, then in month 12, and then in month 24 for the last time.

1. **StepIncrements**

 Rate increments corresponding to StepPeriods. For example .25 .25 .5 means that the rate
will increase by .25, then by .25 again, then finally by .5.

1. **Adjustable**

 This is a Boolean flag, 1 for Adjustable, 0 for fixed rate.
The default is 0.

1. **Margin**

 This is the margin. Default is 0. Only applicable if Adjustable is 1.

1. **InitialRateAdjustmentPeriod**

 Defaults to the specified RateAdjustmentPeriod. Only applicable if Adjustable is 1.

1. **RateAdjustmentPeriod**

 The number of months between rate adjustments.
1 means the rate adjusts every month, 12 means once a year.  Defaults to 0.
Only applicable if Adjustable is 1.

1. **MaximumRateAdjustment**

 The periodic cap, or the maximum the rate can go up in one adjustment period.
Only applicable if Adjustable is 1.

1. **MaximumRate**

 The life time cap. The highest the rate is allowed to be. 0 means no cap. The default is 0.
Only applicable if Adjustable is 1.

1. **MinimumRate**

 The floor. The lowest the rate is allowed to be. Defaults to 0.
Only applicable if Adjustable is 1.

1. **InitialPaymentAdjustmentPeriod**

 Defaults to the PaymentAdjustmentPeriod
Only applicable if Adjustable is 1.

1. **PaymentAdjustmentPeriod**

 The number of months between payment adjustments. The default is 0.
Only applicable if Adjustable is 1.

1. **MaximumPaymentAdjustment**

 This maximum amount the payment can increase in one payment change period.
Only applicable if Adjustable is 1.

1. **MaximumPayment**
Only applicable if Adjustable is 1.

 The absolute maximum amount of the payment.

1. ** RecoveryPeriod **

 The number of months it takes to recover the balance on a defaulting loan. Integer. The default is 0.

1. **PrepaymentRateScenario**

 This specifies the annual prepayment rate on a month by month basis.
Prepayment rates should be in the range 0 (the default) to 100, and are automatically
capped or floored as appropriate. (No error is given; A negative value is forced to 0,
and any value over 100 is forced to 100.)
A prepayment rate of 100 prepays the loan entirely in the specific period.
Thus, to fully prepay a loan in the first month, simply set to the scalar value 100.
To prepay fully in six months, set to the vector 0 0 0 0 0 100.
If the DefaultRateScenario is 100 for a specific period,
then the PrepaymentmentRateScenario is forced to 0 for that specific period,
regardless of what is specified.

1. **DefaultRateScenario**

 This specifies the annual default rate on a month by month basis.
Like the PrepaymentRateScenario, values must be in the range 0 to 100,
and are capped and floored similarly. Also similarly, a default rate of 100 fully
defaults the loan in the relevent period. If the PrepaymentRateScenario is 100
for a specific period, then the DefaultRateScenario is forced to 0 for that specific period,
regardless of what is specified. If BOTH the DefaultRateScenario
and the PrepaymentRateScenario are 100, then the default rate takes precedence and
the prepayment rate is set to 0.

1. **ResecuritizationRateScenario**

 This specifies the annual resecuritization rate on a month by month basis.
The resecuritization rate determines the portion of the performing balance
that is resold into a security at par value. This is useful for modelling loans
that have been bought out of a security with the intent of curing and redelivering,
or loans that have yet to be sold inot a security.


1. **LossSeverityScenario**

 This specifies the loss severity on a month by month basis.

1. **DelinquencyRateScenario30**

 This specifies the 30-day delinquency rate on a month by month basis.

1. **DelinquencyRecoveryRate30**

 This specifies the 30-day delinquency recovery rate. It defaults to 0.
In this case, none of the delinquent interest is recovered, and all
of the delinquent principal is defaulted. A value of 100 indicates
that all of the interest in principal is recovered in the next period.

1. **DelinquencyAdvanceRate30**

 This specifies the 30-day delinquency advance rate. It defaults to 0,
indicating that no principal or interest is advanced by the servicer.
A non-zero delinquency advance rate imples a lower "net delinquency" rate.
For example, a 20% delinquency rate with a 75% advance rate
implies a 5% net rate delinquency rate.

1. **IndexScenario**

 This specifies the index for an ARM on a month by month basis.

1. **OutputNames**

 A list of cashflow detail output names to be kept in the result.
This defaults to all the names. There are many outputs, and with large
data sets it can be useful to restrict the output only to the values
needed for further computation. If output is restricted via this input property,
then the Excel detail cashflow report is not available.

1. **Title**

 A title for reporting. It is useful for this to be a constant to identify
the pricing scenario or company name, but it
may vary by loan or group (repline).

1. **Subtitle**

 A subtitle for reporting. Useful to vary by loan or group (repline)
to identify the particular loan or group, but may also be a constant.

1. **LoanDynamicsOutput**

 This input may be assigned the result of →[loanDynamicsModel]
and is used for reporting purposes only. It does not affect the model in
any way. Specific LDM outputs must be explicitly assigned to specific mortgageCashFlow
inputs. If this input is specified, the outputs (and inputs) of LDM are included
on the Excel detail spreadsheet.

## Time-Series Inputs
Time-series inputs may vary as follows:

Input Structure | One Loan     | Multiple Loans
--------------- | --------     | ------------------------------
Scalar          | Constant     | One constant rate for all loans
Simple          | n/a          | A different constant rate for each loan
Enclosed        | Time-Varying | One time-varying rate vector for all loans
Partitioned     | n/a          | A different time-varying rate vector for each loan


## Outputs
The result of mortgageCashFlow is a property space containing:

1. ** Status **

 A text column containing a status code.
Status is ‘OK’ for successful cash flow generation and price and yield calculuations.
If cash flow is successfully generated, but there is an error computing the price or yield,
then Status is either 'Error computing price' or 'Error computing yield'. Otherwise
Status is an error message specifying where in the cash flow generation process the error occurred.
The AggregateCashFlow output will only include loans that have status equal to
'OK', 'Error computing price', or 'Error computing yield'.

1. **ComputedPrice**

 The computed price given the StatedYield input.
This price is computed on CashFlow.CashFlow (see CashFlow property space below).
Additional prices may be computed using the →[mortgageCashFlowPrice] function.

1. **ComputedYield**

 The computed yield given the StatedPrice input.
This yield is computed on CashFlow.CashFlow (see CashFlow property space below).
Additional yields may be computed using the →[mortgageCashFlowYield] function.

1. ** CashFlow **

 A property space containing monthly cash flow detail for each loan.

1. ** AggregatedCashFlow **

 A DataTable containing aggregated cash flows. Columns are summed or
weighted-averaged by BeginningBalance as appropriate.

Some outputs are non time-varying per mortgage (e.g. price and yield). Most output columns are
time-varying per mortgage (e.g. ScheduledPrincipal and StartingBalance), and
are contained in the CashFlow property space.
If a single loan is priced, the time-varying outputs are simple (a list)
and the non time-varying columns are scalar. If multiple loans are priced,
the time-varying output columns are partitioned (list of lists)
and the non time-varying outputs are simple.

## The CashFlow Property Space Output

The outputs are listed below in order of their computation,
as an aid to undertanding the model. The first few outputs like
PrepaymentRate and DefaultRate are simply the corresponding inputs,
adjusted for term and correctness, to show the precise value in effect per period.

The main outputs are near the end, including Principal, Interest, and CashFlow.
CashFlow is used for ComputedYield and ComputedPrice.

1. **ID**

 The ID input, replicated by the length of the cash flow.

1. **Period**

 Time in months.

1. **PrepaymentRate**

 The prepayment rate in effect per period.

1. **DefaultRate**

 The default rate in effect per period.

1. **LossSeverity**

 The loss severity in effect per period.

1. **DelinquencyRate30**

 The 30-day delinquency rate in effect per period.

1. **Index**

 The index in effect per period.

1. **Rate**

 The rate in effect for each period. Will be constant if a fixed rate loan.
If an ARM, will vary according to ARM inputs and the IndexScenario input.
If a step loan, will vary according to StepPeriods and StepIncrements.

1. **BeginningPerformingBalance**

 The amortized performing balance at the beginning of the period,
given prepayments and defaults.

1. ** BeginningNonperformingBalance **

 The nonperforming balance at the beginning of the period, given
prepayments and defaults.

1. ** PerformingPrepaidPrincipal **

 Prepaid principal on the performing balance.

1. ** NonperformingPrepaidPrincipal **

 Prepaid principal on the nonperforming balance.

1. ** PerformingDefaultedPrincipal **

 Defaulted principal on the performing balance.

1. ** NonperformingDefaultedPrincipal **

 Defaulted principal on the nonperforming balance.

1. ** ResecuritizedPrincipal **

 The portion of the performing balance that is redelivered into a security
by the resecuritization rate.

1. **PenaltyFee**

 Prepayment penalty fee. PerformingPrepaidPrincipal multiplied by PenaltyFee rate input.

1. **BeginningScheduledPrincipal**

 Scheduled principal, computed on BeginnningPerformingBalance less PerformingDefaultedPrincipal.

1. **ScheduledInterest**

 Scheduled gross interest, computed on BeginnningPerformingBalance less PerformingDefaultedPrincipal.

1. **ScheduledPayment**

 The sum of ScheduledPrincipal and ScheduledInterest.

1. ** ScheduledServiceFee**

 Service fee computed on BeginningPerformingBalance less PerformingDefaultedPrincipal.

1. **ScheduledNetInterest**

 ScheduledInterest less ScheduledServiceFee

1. **DelinquentNetInterest**

 ScheduledNetInterest multipled by the net delinquency rate
(DelinquencyRate30 adjusted by DelinquencyAdvanceRate30.)

1. **DelinquentNetInterestRecovered**

 DelinquentNetInterest, shifted out one month (for 30 day delinquencies)
multiplied by DelinquentRecoveryRate30.

1. **DelinquentPrincipal**

 ScheduledPrincipal multipled by the net delinquency rate
(DelinquencyRate30 adjusted by DelinquencyAdvanceRate.)

1. **DelinquentPrincipalRecovered**

 DelinquentPrincipal, shifted out one month (for 30 day delinquencies)
multiplied by DelinquentRecoveryRate30.

1. **DelinquentPrincipalLosses**

 DelinquentPrincipalRecovered subracted from the previous period's DelinquentPrincipal.

1. **DelinquentServiceFee**

 ScheduledServiceFee multipled by the net delinquency rate
(DelinquencyRate30 adjusted by DelinquencyAdvanceRate.)

1. **DelinquentServiceFeeRecovered**

 DelinquentService, shifted out one month (for 30 day delinquencies)
multiplied by DelinquentRecoveryRate30.

1. **PerformingLiquidatedPrincipal**

 Liquidated principal due to defaults on performing balance;
PerformingDefaultedPrincipal shifted out in time
according to RecoveryPeriod.

1. **NonperformingLiquidatedPrincipal**

 Liquidated principal due to defaults on nonperforming balance;
NoPerformingDefaultedPrincipal shifted out in time
according to the RecoveryPeriod.

1. **PerformingLosses**

 PerformingLiquidatedPrincipal multiplied by the loss severity.
Note that the loss severity is also time-shifted by the RecoveryPeriod.

1. **NonperformingLosses**

 NonperformingLiquidatedPrincipal multiplied by the loss severity.
Note that the loss severity is also time-shifted by the RecoveryPeriod.

1. **OtherLiquidatedPrincipal**

   Other nonperforming related liquidated principal. NonperformingPrepaidPrincipal, plus the
final period's BeginningNonperformingBalance less the final period's NonperformingLiquidatedPrincipal.
This liquidated principal is recovered in full. There are no losses associated with it.

1. **PerformingRecoveredPrincipal**

 Recovered principal related to the performing balance; PerformingLiquidatedPrincipal less PerformingLosses.

1. **NonperformingRecoveredPrincipal**

 Recovered principal related to the nonperforming balance;
OtherLiquidatedPrincipal plus NonperformingLiquidatedPrincipal, less NonperformingLosses.

1. **RecoveredPrincipal**

 Total Recovered principal; PerformingRecoveredPrincipal plus NonperformingRecoveredPrincipal.

1. **LiquidatedPrincipal**

 Total liquidated principal; The sum of PerformingLiquidatedPrincipal, NonperformingLiquidatedPrincipal,
OtherLiquidatedPrincipal, and DelinquentPrincipalLosses.

1. **Losses**

 Total principal losses; LiquidatedPrincipal less RecoveredPrincipal.
May also be computed as the sum of PerformingLosses, NonperformingLosses, and DelinquentPrincipalLosses.

1. **PerformingPrincipal**

 The sum of BeginningScheduledPrincipal, PerformingPrepaidPrincipal, and PerformingRecoveredPrincipal.
This value is used to back into the performing balance.

1. **ScheduledPrincipal**

 The sum of (BeginningScheduledPrincipal - DelinquentPrincipal) and DelinquentPrincipalRecovered.

1. **UnscheduledPrincipal**

 The sum of PerformingPrepaidPrincipal and RecoveredPrincipal. Note that Nonperforming prepaid
principal is already included in RecoveredPrincipal.

1. **Principal**

 The sum of ScheduledPrincipal and UnscheduledPrincipal.

1. **Interest**

 The sum of (ScheduledNetInterest less DelinquentNetInterest) and DelinquentNetInterestRecovered.

1. **ServiceFee**

 The sum of (ScheduledServiceFee less DelinquentServiceFee) and DelinquentServiceFeeRecovered.

1. **CashFlow**

 The sum of Principal, Interest and PenaltyFee.

1. **PerformingBalance**

 The (ending) balance implied by the sum of PerformingPrincipal and PerformingLosses.
This is computed with a reverse running sum.

1. **NonperformingBalance**

 The sum of DelinquentPrincipal and the one-period shifted BeginningNonperformingBalance.

1. **Balance**

 The sum of PerformingBalance and NonperformingBalance.

1. **DelinquentBalance**

 Balance multiplied delinquency rate.

## Example 1
~~~
      P=newPropertySpace 0
      P.Rate=5
      P.Balance=100000
      P.Term=12
      P.AmortizationTerm=120
      P.StatedPrice=97
      R=mortgageCashFlow P
      R.ComputedYield
┌───────────────┐
│8.2507683912588│
└Float──────────┘
      R.ScheduledPrincipal
┌─────────────────┐
↓  643.98848572408│
│  646.67177108126│
│  649.36623679412│
│  652.07192944743│
│  654.78889582014│
│  657.51718288602│
│  660.25683781473│
│  663.00790797225│
│  665.77044092216│
│  668.54448442608│
│  671.33008644446│
│92766.685740667  │
└Float────────────┘

     5 mortgageCashFlowPresentValue R.TotalCash
┌──────┐
│100000│
└Float─┘
~~~

## Example 2

~~~
      P=newPropertySpace 0
      P.Rate=5
      P.Balance=100000
      P.Term=12
      P.AmortizationTerm=120
      P.DefaultRateScenario=8
      P.PrepaymentRateScenario=6
      P.StatedYield=7
      R=mortgageCashFlow P
      R.ComputedPrice
┌───────────────┐
│98.227253612965│
└Float──────────┘
      R.LiquidatedPrincipal
┌───────────────┐
↓692.43826282994│
│679.67692500085│
│667.10461075093│
│654.71876055419│
│642.51684798578│
│630.49637930503│
│618.65489304352│
│606.98995959835│
│595.49918083038│
│584.1801896675 │
│573.03064971283│
│562.04825485775│
└Float──────────┘
      R.PrepaidPrincipal
┌───────────────┐
↓510.98924213667│
│501.53722590049│
│492.22539556986│
│483.05185174837│
│474.01471961195│
│465.11214859922│
│456.34231210563│
│447.7034071815 │
│439.19365423371│
│430.81129673113│
│422.55460091372│
│  0            │
└Float──────────┘
 ~~~


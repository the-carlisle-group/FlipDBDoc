# Mortgage Cash Flows

FlipDB includes a set of functions for generating and analyzing residential mortgage cash flows.
The primary function is →[*.mortgageCashFlow], which provides a wide range of
inputs and comprehensive set of outputs. In addition there is →[*.mortgageCashFlowPrice],
→[*.mortgageCashFlowYield], and →[*.mortgageCashFlowPresentValue], for computing prices, yields,
and present values respectively.

The mortgageCashFlow function takes a property space as input,
and returns a property space as output. A simple example will make this clear.
Here is the Columns property of an ungrouped query for loan level pricing:

|. | Name             | Expression
|--| -----------------| ----------------------
|1|`Input`           |`newPropertySpace 0`
|2 |`Input.Balance`   |`CurrentBalance`
|3 |`Input.Term`      |`MaturityDate monthsDifference AsOfDate`
|4 |`Input.Rate`      |`Rate`
|5 |`Input.ServiceFee`|`.5`
|6 |`Input.StatedPrice`|`97`
|7 |`Output`          |`mortgageCashFlow Input`
|8 |`CashFlow`        |`Output.CashFlow`
|9 |`ComputedYield`   |`3 round Output.ComputedYield`
|10|`TotalInterest`   |`sum CashFlow.ScheduledInterest`
|11|`ComputedYield2`  |`102 Input.Balance mortgageCashFlowYield CashFlow.CashFlow`
|12|`PVofServicing`   |`4 mortgageCashFlowPresentValue  (-CashFlow.ServiceFeeIncome)`

The first line establishes a new property space to collect
the inputs to the cash flow function.

The next five lines [2-6] define some inputs. An input
may be the name of a column, an expression, or a constatnt.
There are over 30 named inputs, all which have default values
so you only need to specify the ones you want.

Once the desired inputs arguments are defined in the property space,
the mortgageCashFlow function is called on line 7,
with the property space as its argument.

The explicit result of the function is also a property space,
which contains all of the named outputs of the function. One of the
named outputs (CashFlow) is itself a property space. We pull this
up a level on line 8 to make it easier to access later.
On line 9 we extract and round the ComputedYield output,
to return it in the query result.

On line 10 we extract and sum the ScheduledInterest output.
Note that aggregation is required as ScheduledInterest is
a partioned column - there is a list of monthly interest payments
for each loan.

On line 11, we compute an additional yield for a different price.
The cashFlow function itself computes one yield based on the StatedPrice
input and the TotalCash output. However, the mortgageCashFlowYield function
allows computing multiple yields on any flows or functions of those flows.
Similar functionality exists for price given yield.

On line 12 we compute the present value of the service fee income at 4%.'

For grouped queries the inputs must be aggregated, and
potentially rounded, and specified in the Measures property of the query.
For example:


. |  Name           | Expression
- | -------------   | ------------------
1 | `Input`         | `newPropertySpace 0`
2 | `Input.Balance` | `sum CurrentBalance`
3 | `Input.Term`    | `round average MaturityDate monthsDifference AsOfDate`
4 | `Input.Rate`    | `weightedAverage Rate CurrentBalance`
5 | `CashFlow`      | `mortgageCashFlow Input`
6 | `ComputedYield` | `95 Input.Balance mortgageCashFlowYield CashFlow.TotalFlow`

On line 1, a new property space is defined, as in the above example.
On line 2, however, we must sum the current balance column. Similarly
for lines 3 and 4, we must aggregate the inputs as the query is grouped,
and we are running cash flows on each group, not on each mortgage.

## Default, Missing, and Misspelled Inputs

Note that no error is signaled when misspelling the name of an input
specified in the property space (or omiting it entirely).
The mortgageCashFlow function allows for additional names and values
to be in the input property space for informational purposes on reports.
Thus if you mistype, say, AmortizationTerm as AmortztonTerm, the default value
of 360 is used, and no error is reported.

There are many inputs, and virtually all them are optional and have
default values. Thus it is important to verify that the function is in fact
using the inputs you think you have specfied. The Excel detail report
will highlight the differences between inputs that are specified, and inputs that
are not specified and for which default values are used.

## Time-Varying Inputs
There are five "scenario" inputs to the cashflow function for prepayment
rates, default rates, loss severity, and delinquency, and index. These
inputs may be scalar, simple, enclosed, or partitioned.

These inputs must be adjusted for loan age upon or before assignment; the mortageCashFlow
function itself has no knowledge of age and makes no adjustment for it.

## Andrew Davidson & Company's LoanDynamics Model
FlipDB provides access to the LoanDynamics Model (LDM) from
Andrew Davidson & Company (AD&Co) through the →[*.loanDynamicsModel] function.
Outputs from LDM may then be used to specify inputs to the mortgageCashFlow function.
Outputs from LDM that are useful for inputs to mortgageCashFlow include
prepayment rate vectors, default rate vectors, and loss severity vectors.
You will need a licence (and key) from AD&Co in order to use this feature.

FlipDB does not install any data files from AD&Co. You must have
or create a folder that contains them. This folder may be named and located
as desired (you will need to specify this path later),
but it must contain the following subfolders and files:

1. **DataFile**

 This subfolder contains the LDM data files. AD&Co updates this monthly.
It must also contain your license key file "adco_lic.key".

1. **IO**

 This folder is used for optional additional input and output. It may be empty.

The loanDyamicsModel takes a property space as an argument.
It must include the minimum necessary LDM inputs:

. |  Name                    | Expression
- | -------------            | ------------------
1 | `LDM`                    | `newPropertySpace 0`
2 | `LDM.ADCOFolder`         | `"c:ADCO"`
3 | `LDM.FirstForecastYear`  | `2016`
4 | `LDM.FirstForecastMonth` | `9`
5 | `LDM.WacIsFixed`         | `1`
6 | `LDM.Age`                | `Age`
7 | `LDM.RemainingTerm`      | `RemTerm`
8 | `LDM.OriginalTerm`       | `OrigTerm`
9 | `LDMOutput`              | `loanDynamicsModel LDM`

The output from the loanDyamicsModel function is also a property space,
containing all of the outputs provided by LDM. These outputs are simply
passed though as-is, with no transformations. To use them as inputs for
mortgageCashFlow they may need to be modified. For example, below the LDM
output MRR is converted to annual CPR, and LossSeverity is converted to
a percentage:

|. | Name                     | Expression
|--| -----------------        | ----------------------
|1|`I`                       |`newPropertySpace 0`
|2 |`I.Balance`               |`CurrentBalance`
|3 |`I.Term`                  |`RemTerm`
|4 |`I.Rate`                  |`Rate`
|5 |`I.PrepaymentRateScenario`|`smmToCPR LDMOutput.MRR`
|6 |`I.LossServerityScenario` |`100 times LDMOutput.LossSeverity`
|7 |`I.LoanDynamicsOutput`    |`LDMOutput`
|8 |`CashFlow`                |`mortgageCashFlow I`

Note on line 7 that the entire LDM output property space is set
as an input to mortgageCashFlow. This does not affect the cashflow model in any way;
It ties the two models together only for displaying data in the Excel detail spreadsheet.

The LoanDynamics model has many inputs, and the input names and interpretations may be
different when compared to the FlipDB cash flow function and its inputs. These are
entirely separate models, and the inputs to each model are strictly separated.

For this reason, and for readability and flexibility, it is convenient to put the LDM input specification
into its own expression set and to call it using →[*.evaluateExpressionSet].

Every value in the LDM input property space is assumed to be an input to the LDM model,
If a name is misspelled, or not an LDM input, or an invalid value,
an error is signalled.

Please refer to the AD&Co documentation for details on the inputs.


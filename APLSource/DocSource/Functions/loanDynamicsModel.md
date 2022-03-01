# loanDynamicsModel

Type:{.prefix}

Misc{.info}

Categories:{.prefix}

Financial{.info}

Run the Andrew Davidson & Co. (AD&Co) Loan Dynamics Model (LDM).{.purpose}

A license (and key) from AD&Co is needed to use this function.

See also →[mortgageCashFlow], →[mortgageCashFlowYield],
→[mortgageCashFlowPrice], →[mortgageCashFlowPresentValue], →[smmToCPR] 

~~~
R=loanDynamicsModel X
~~~

X is a property space containing appropriate inputs for LDM.
R is a property space containing all of the LDM outputs, and a Status field
provided by FlipDB.

Output from the loanDynamicsModel function may be used as inputs
for the mortgageCashFlow function.

This function will signal an error if FlipDB cannot initialize or run the model
for any reason, or if LDM fails for every loan due to invalid or missing input.
If the model runs successfully for some loans,
but fails on others, no error is signalled. The Status field can be
used to see which loans run and which do not.

This function may be used in ungrouped or grouped queries,
providing loan level or repline data respectively.
If used in a grouped query, then inputs must be aggregrated.

## Inputs
The LoanDyamics model accepts many inputs. The minimum of 6 inputs necessary
to run the model are described here. See the Ad-Co documentation for further
details. In addition, FlipDB requires a folder to be specied for the
location of the LDM data files and license key file. FlipDB also
provides an optional input named OutputNames for limiting the ADCO output.

1. ** ADCOFolder **

 This is the name of the folder that holds the LDM data files and the license key file.
It must contain a DataFile subfolder.

1. **OutputNames**

 A list of ADCO outputs to return. If not specified, or if
 an empty list, all ADOC outputs are returned. Note that there
 are over 250 outputs, and if only a few are needed, it is worth
 explicitly specifying them to improve performance.

1. **FirstForecastYear**

 An integer year.

1. **FirstForecastMonth**

 An integer month.

1. **WacIsFixed**

 A boolean, 1 for fixed, 0 for adjustable.
(This is the opposite of the Adjustable input of mortgageCashflow)

1. **Age**

 Integer. The age of the loan.

1. **RemainingTerm**

 Integer. The remaining term of the loan.
(Corresponds to Term in mortgageCashFlow inputs)

1. **OriginalTerm**

 Integer. The original amortization term of the loan.

Some AdCo inputs are time-varying. These must provided as enclosed
or partitioned columns. FlipDB will truncate or extend time-varying
inputs to correspond to the remaining term.

AdCo input names are not always valid FlipDB names. As of this writing
there are at least two:  2YrForecast and 10YrForecast. FlipDB maps AdCo
names with leading digits to valid FlipDB names by moving the digits to the
end of the name. So 2YrForecast is mapped to YrForecast2 and 10YrForecast is
mapped to YrForecast10. The valid FlipDB names should be used when
specifying inputs. Similar issues arise with output names. FlipDB converts
them to valid FlipDB names in the same way.

## Outputs
The result of loanDynamicsModel is a property space containing all of the outputs
provided by LDM. In addition, FlipDB provides an output named "Status" that reports
'OK' for a successful execution, or an error message passed through from AD&Co.


With the exception of time series fields, which are adjusted to
correspond with first forecast year and month,
FlipDB does not do any transformation of the LDM outputs; they are
passed through as is. They may require some transformation in order to be
properly used as inputs to other FlipDB functions.
See the AD&Co documentation for a full list and description
of all of the outputs.

Some outputs are non time-varying per mortgage.
Some outputs are time-varying per mortgage (e.g. MDR and LossSeverity).
If a single loan is priced, the time-varying outputs are simple (a list)
and the non time-varying columns are scalar. If multiple loans are priced,
the time-varying output columns are partitioned (list of lists)
and the non time-varying outputs are simple.

Three of the primary outputs used as inputs to mortgageCashFlow are:

1. **MRR**

 A time-series output used as PrepaymentRateScenario. MRR must be annualized first
using the →[smmToCPR] function.

1. **MDR**

 A time-series output used as DefaultRateScenario. MDR must be annualized first
using the →[smmToCPR] function.

1. **LossSeverity**

 A time-series output used as LossSeverity. LossSeverity is provided in decimal
form by LDM, and must by converted to a percentage (multiplied by 100).

### Examples
This example uses the minimum number of inputs to run the model:

~~~
      P=newPropertySpace 0
      P.ADCOFolder←='c:portsadcolipdb_adco'
      P.FirstForecastYear=2012
      P.FirstForecastMonth=9
      P.WacIsFixed=1
      P.OriginalTerm=359
      P.RemainingTerm=321
      P.Age=32
      R=loanDynamicsModel P
      R.Status
┌───────┐
│OK     │
└Char(2)┘
      R.MRR
┌──────────────────┐
↓0.0055354502023933│
│0.0057265627365561│
│0.0055034381344095│
│0.0057881061943596│
│0.0061031110323175│
│0.0060401808390437│
│0.0064250605675414│
│0.0067857262808221│
│0.008040201628693 │
│0.0086951764088216│
│...               │
└Float─────────────┘

~~~

Building on the above example, we can specify a time series input. Note that times
series inputs must be enclosed (or partitioned when running multiple loans). Note
further that times series inputs are automatically truncated or extended to
correspond to the remaining term:

~~~
      P.YrForecast2=enclose  3 3.5 4 4.5 5 5.5 6 7
      R=loanDynamicsModel P
      R.YrForecast2
┌─────┐
↓3    │
│3.5  │
│4    │
│4.5  │
│5    │
│5.5  │
│6    │
│7    │
│7    │
│7    │
│...  │
└Float┘
~~~



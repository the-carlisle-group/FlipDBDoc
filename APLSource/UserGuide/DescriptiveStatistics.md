# Descriptive Statistics

A data column can be described graphically, with a table, or with summary statistics. Graphics
include bar charts, pie charts and histograms. Tables include frequency distributions, relative
frequencies, and cumulative frequencies. Summary statistics include measures of central tendency,
spread, position and shape. FlipDB provides a rich set of functions and operators for statistical
computations. When these statistical functions and operators require a data set as input, the user
may substitute  a frequency distribution or summary statistics (e.g. mean, standardDeviation and
sampleSize). Some loss of data may occur in this instance, so results may vary slightly.

## Frequency Distributions

There are two broad categories of data:  qualitative and quantitative. An example of qualitative
data is COLORS:

~~~
      COLORS= 'Red,Blue,Blue,Green,Yellow,Yellow,Red,Red'
~~~

The simplest way to describe qualitative data is with a frequency distribution. The →frequency
function produces a table of the unique values in the argument:

~~~
      frequency COLORS
─────────────────────────────────────────────────────────────────
 ┌Values─┐  ┌Count┐  ┌Percent┐  ┌RunningCount┐  ┌RunningPercent┐
 ↓Red    │  ↓3    │  ↓37.5   │  ↓3           │  ↓ 37.5         │
 │Blue   │  │2    │  │25     │  │5           │  │ 62.5         │
 │Green  │  │1    │  │12.5   │  │6           │  │ 75           │
 │Yellow │  │2    │  │25     │  │8           │  │100           │
 └Char(6)┘  └Int8─┘  └Float──┘  └Int8────────┘  └Float─────────┘
── 4 rows by 5 columns ──────────────────────────────────────────
~~~

In addition to the unique values, the result of the frequency function contains frequency of each
value (Count), the relative frequency or proportion (Percent), the cumulative frequency
(RunningCount) and the relative cumulative frequency (RunningPercent).





****** Add Example with map function to range qualitative data, making fewer groups.

The ages of workers in a small company is an example of a quantitative data set:

~~~
      AGE=33 34 35 36 37 37 37 38 39 39 40 41 41 42 48
~~~

We can also describe quantitative data with a frequency distribution:

~~~
      frequency AGE
────────────────────────────────────────────────────────────────────────────
 ┌Values┐  ┌Count┐  ┌Percent─────────┐  ┌RunningCount┐  ┌RunningPercent───┐
 ↓33    │  ↓1    │  ↓ 6.6666666666667│  ↓1           │  ↓  6.6666666666667│
 │34    │  │1    │  │ 6.6666666666667│  │2           │  │ 13.333333333333 │
 │35    │  │1    │  │ 6.6666666666667│  │3           │  │ 20              │
 │36    │  │1    │  │ 6.6666666666667│  │4           │  │ 26.666666666667 │
 │37    │  │3    │  │20              │  │7           │  │ 46.666666666667 │
 │38    │  │1    │  │ 6.6666666666667│  │8           │  │ 53.333333333333 │
 │39    │  │2    │  │13.333333333333 │  │10          │  │ 66.666666666667 │
 │40    │  │1    │  │ 6.6666666666667│  │11          │  │ 73.333333333333 │
 │41    │  │2    │  │13.333333333333 │  │13          │  │ 86.666666666667 │
 │42    │  │1    │  │ 6.6666666666667│  │14          │  │ 93.333333333333 │
 │48    │  │1    │  │ 6.6666666666667│  │15          │  │100              │
 └Int8──┘  └Int8─┘  └Float───────────┘  └Int8────────┘  └Float────────────┘
── 11 rows by 5 columns ────────────────────────────────────────────────────
~~~

Numeric and data values can be grouped into fewer buckets by applying the →range function before
computing the frequency. In the following example, we count the ages in groups of three:

~~~
      frequency 3 range AGE
 ┌────┐  ┌────┐  ┌─────────────────┐  ┌────┐  ┌────────────────┐
 ↓33  │  ↓3   │  ↓0.2              │  ↓3   │  ↓0.2             │
 │36  │  │5   │  │0.33333333333333 │  │8   │  │0.53333333333333│
 │39  │  │5   │  │0.33333333333333 │  │13  │  │0.86666666666667│
 │42  │  │1   │  │0.066666666666667│  │14  │  │0.93333333333333│
 │48  │  │1   │  │0.066666666666667│  │15  │  │1               │
 └Int8┘  └Int8┘  └Float────────────┘  └Int8┘  └Float───────────┘
~~~

## Measures of Central Tendency

We often wish to describe the center of the data. The most obvious is the mean or average:

~~~
      mean AGE
┌───────────────┐
│38.466666666667│
└Float──────────┘
~~~

The median is the value where half of the remaining values are smaller and half are larger:

~~~
      median AGE
┌────┐
│38  │
└Int8┘
~~~

The mode is the most frequently-occurring value:

~~~
      mode AGE
┌────┐
│37  │
└Int8┘
~~~

### Measures of Position

We sometimes wish to locate data in positions other than the center.  For example the smallest or
largest values:

~~~
      min AGE
┌────┐
│33  │
└Int8┘
      max AGE
┌────┐
│48  │
└Int8┘
~~~

We can locate other positions within the data, such as the 25th percentile also known as the first
quartile or Q1.  This is the value which is greater than 25% of all the other values.  The 75th
percentile is also known as the Third Quartile or Q3.

~~~
      25 percentile AGE
┌────┐
│36  │
└Int8┘
      75 percentile AGE
┌────┐
│41  │
└Int8┘
~~~

The min,Q1,median,Q3 and max are known as the Five-Number Summary  The five-number summary for AGE
is:  33,36,38,41,48

### Measures of Spread

Data can be clustered tightly or spread out.   One way to measure the spread is to calculate the
range of the data:

~~~
      (max AGE)-min AGE
┌────┐
│15  │
└Int8┘
~~~

Unfortunately this method is very sensitive to outliers.  The range would only be 9 if we removed
the 48-year old from the group.  The inter-quartile range takes the difference between Q3 and Q1
and is less sensitive to outliers:

~~~
      interQuartileRange AGE
┌────┐
│5   │
└Int8┘
~~~

Another way to measure spread is to calculate the average squared distance from the mean. This is
known as the variance:

~~~
      variance AGE
┌───────────────┐
│13.838095238095│
└Float──────────┘
~~~

The square root of the variance is the standard deviation.  This is a more practical measure
because it is in the same units as the mean.

~~~
      standardDeviation AGE
┌───────────────┐
│3.7199590371529│
└Float──────────┘
~~~

### Measures of Shape

Skewness measures whether a data set is symmetric, or is skewed to the left or right.  AGE has a
positive skew due to the 48-year old outlier:

~~~
      skewness AGE
┌───────────────┐
│1.0058590492254│
└Float──────────┘
~~~

When we remove the outlier, the skewness is close to zero showing a fairly symmetric data set:

~~~
      skewness -1 drop AGE
┌─────────────────┐
│-0.16418560651699│
└Float────────────┘
~~~

## Data Representation

We can represent data in three ways:  as a Data Column, a Frequency Table or as Summary Statistics.
Each of these three representations can be used as input to statistical functions.   To convert
a Data column to a Frequency Table, use the →frequency function.   To convert a Data column to
Summary Statistics, use the →stats function.


# independent

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

Chi-Square test for independence.

⍎⍎#.FlipDB.flipdb.TamStatInt.HelpMsg 0 ⍎⍎

~~~
R=independent X Y
~~~

X and Y must be conformable Integers or Characters.

R is a PropertySpace that includes:

* **DegreesOfFreedom**
* **Factors**
* **P** - P Value.
* **Table** is a DataTable including the following columns:
    * **Factor1**
    * **Factor2**
    * **Observed**
    * **Expected**
    * **Difference**
    * **ChiSquare** - ChiSquare contribution
* **TestStatistic**
* **Type**

## Examples

~~~
      S=TheServer
      State=toChar S.Get 'Databases/LoanSample/Loan/State'
      PropTyp=toChar S.Get 'Databases/LoanSample/Loan/PropertyType'
      R=independent State PropTyp
      R
┌PropertySpace──────────────────┐
│ Name                Type      │
│ ----------------    --------- │
│ DegreesOfFreedom    Integer   │
│ Factors             Char      │
│ P                   Float     │
│ Table               DataTable │
│ TestStatistic       Float     │
│ Type                Char      │
└───────────────────────────────┘
      R.Table
────────────────────────────────────────────────────────────────────────────────────────
 ┌Factor1┐  ┌Factor2──────┐  ┌Observed┐  ┌Expected┐  ┌Difference┐  ┌ChiSquare─────────┐
 ↓FL     │  ↓Condo        │  ↓100     │  ↓230     │  ↓(130)     │  ↓ 73.478260869565  │
 │FL     │  │Other        │  │0       │  │20      │  │(20)      │  │ 20               │
 │FL     │  │Single Family│  │900     │  │750     │  │150       │  │ 30               │
 │GA     │  │Condo        │  │100     │  │69      │  │31        │  │ 13.927536231884  │
 │GA     │  │Other        │  │0       │  │6       │  │(6)       │  │  6               │
 │GA     │  │Single Family│  │200     │  │225     │  │(25)      │  │  2.7777777777778 │
 │MD     │  │Condo        │  │100     │  │92      │  │8         │  │  0.69565217391304│
 │MD     │  │Other        │  │0       │  │8       │  │(8)       │  │  8               │
 │MD     │  │Single Family│  │300     │  │300     │  │0         │  │  0               │
 │NC     │  │Condo        │  │100     │  │69      │  │31        │  │ 13.927536231884  │
 │NC     │  │Other        │  │0       │  │6       │  │(6)       │  │  6               │
 │NC     │  │Single Family│  │200     │  │225     │  │(25)      │  │  2.7777777777778 │
 │NJ     │  │Condo        │  │200     │  │138     │  │62        │  │ 27.855072463768  │
 │NJ     │  │Other        │  │0       │  │12      │  │(12)      │  │ 12               │
 │NJ     │  │Single Family│  │400     │  │450     │  │(50)      │  │  5.5555555555556 │
 │SC     │  │Condo        │  │0       │  │46      │  │(46)      │  │ 46               │
 │SC     │  │Other        │  │0       │  │4       │  │(4)       │  │  4               │
 │SC     │  │Single Family│  │200     │  │150     │  │50        │  │ 16.666666666667  │
 │VA     │  │Condo        │  │1,700   │  │1,656   │  │44        │  │  1.1690821256039 │
 │VA     │  │Other        │  │200     │  │144     │  │56        │  │ 21.777777777778  │
 │VA     │  │Single Family│  │5,300   │  │5,400   │  │(100)     │  │  1.8518518518519 │
 │       │  │Total        │  │10,000  │  │10,000  │  │0         │  │314.46054750403   │
 └Char(2)┘  └Char(13)─────┘  └Int16───┘  └Int16───┘  └Int16─────┘  └Float─────────────┘
── 22 rows by 6 columns ────────────────────────────────────────────────────────────────
      R.P
┌───────────────────┐
│5.5511151231258E-16│
└Float──────────────┘
      R.P lt .001
┌───────┐
│1      │
└Boolean┘
~~~

~~~
      Gender='F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,F,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M,M'
      EyeColor='Green,Hazel,Green,Blue,Purple,Blue,Hazel,Green,Hazel,Green,Blue,Brown,Brown,Brown,Blue,Hazel,Blue,Green,Blue,Brown,Brown,Blue,Hazel,Green,Green,Blue,Green,Green,Brown,Green,Blue,Brown,Blue,Green,Brown,Purple,Brown,Brown,Brown,Hazel,Purple,Blue,Blue,Brown,Blue,Blue,Hazel,Brown,Blue,Hazel'
      R=independent EyeColor Gender
      R.Table
─────────────────────────────────────────────────────────────────────────────────
 ┌Factor1┐  ┌Factor2┐  ┌Observed┐  ┌Expected┐  ┌Difference┐  ┌ChiSquare────────┐
 ↓Blue   │  ↓F      │  ↓7       │  ↓7.5     │  ↓(0.5)     │  ↓0.033333333333333│
 │Blue   │  │M      │  │8       │  │7.5     │  │0.5       │  │0.033333333333333│
 │Brown  │  │F      │  │5       │  │6.5     │  │(1.5)     │  │0.34615384615385 │
 │Brown  │  │M      │  │8       │  │6.5     │  │1.5       │  │0.34615384615385 │
 │Green  │  │F      │  │7       │  │5.5     │  │1.5       │  │0.40909090909091 │
 │Green  │  │M      │  │4       │  │5.5     │  │(1.5)     │  │0.40909090909091 │
 │Hazel  │  │F      │  │5       │  │4.0     │  │1.0       │  │0.25             │
 │Hazel  │  │M      │  │3       │  │4.0     │  │(1.0)     │  │0.25             │
 │Purple │  │F      │  │1       │  │1.5     │  │(0.5)     │  │0.16666666666667 │
 │Purple │  │M      │  │2       │  │1.5     │  │0.5       │  │0.16666666666667 │
 │       │  │Total  │  │50      │  │50.0    │  │0.0       │  │2.4104895104895  │
 └Char(6)┘  └Char(5)┘  └Int8────┘  └Dec(1)──┘  └Dec(1)────┘  └Float────────────┘
── 11 rows by 6 columns ─────────────────────────────────────────────────────────
      R.TestStatistic
┌───────────────┐
│2.4104895104895│
└Float──────────┘
~~~


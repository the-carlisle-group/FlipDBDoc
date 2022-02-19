# toCSV

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=[Y] toCSV X
~~~

X is any column but Blob. R is X with items formatted and gathered into a comma separated string.
If Y is present it is a single character to be used as separator instead of the comma. If a token
contains a separator the data is enclosed by double-quotes. If the token contains double quotes
those are doubled.

## Examples

~~~
      a='California,New York,New Jersey'
      a
┌──────────┐
↓California│
│New York  │
│New Jersey│
└Char(10)──┘
      toCSV a
┌──────────────────────────────┐
│California,New York,New Jersey│
└Char(30)──────────────────────┘
      b=enclose 'NY,CT,NJ' 'CA,NV' 'TX,LA,NM,FL'
      b
┌─────────────┐
↓[NY,CT,NJ]   │
│[CA,NV]      │
│[TX,LA,NM,FL]│
└Char(2)──────┘
      toCSV b
┌───────────┐
↓NY,CT,NJ   │
│CA,NV      │
│TX,LA,NM,FL│
└Char(11)───┘
~~~


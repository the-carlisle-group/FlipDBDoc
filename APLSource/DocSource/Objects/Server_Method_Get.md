# Get Method

Applies to:{.prefix}

→[##.##.Server]{.info}

The Get method returns a resource from the path portion of its URL. The schema and domain should
not be included in the argument.

The argument is composed of 1 item:

|-|-|
|1|Name|String|

The result is composed of 1 item:

|-|-|
|1|Value|Instance|

For example, to get the Parts table from the Suppliers and Parts database:

~~~
      s.Get '/Databases/SandP/P'
── SandP.P ────────────────────────────────────────────
 ┌PNO────┐  ┌PNAME──┐  ┌COLOR──┐  ┌WEIGHT┐  ┌CITY────┐
 ↓P1     │  ↓Nut    │  ↓Red    │  ↓12    │  ↓London  │
 │P2     │  │Bolt   │  │Green  │  │17    │  │Paris   │
 │P3     │  │Screw  │  │Blue   │  │17    │  │Oslo    │
 │P4     │  │Screw  │  │Red    │  │14    │  │London  │
 │P5     │  │Cam    │  │Blue   │  │12    │  │Paris   │
 │P6     │  │Cog    │  │Red    │  │19    │  │London  │
 └Char(2)┘  └Char(5)┘  └Char(5)┘  └Int8──┘  └Char(10)┘
── 6 rows by 5 columns ────────────────────────────────
~~~


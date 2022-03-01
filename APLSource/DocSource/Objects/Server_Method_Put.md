# Put Method

Applies to:{.prefix}

→[##.##.Server]{.info}

The Put method saves a resource on the server.

The argument is composed of 2 items:

|-|-|
|1|Name|String|
|2|value|Instance|

The result is composed of 1 item:

|-|-|
|1|Value|Instance|

The schema and domain should not be included in the Name item. For example:

~~~
      s=Scripting.Server
      dt=('Name' 'California,ZeroBalance' )  ('Expression' 'State in "CA",Balance ==0')
      r=s.Put '/Objects/MyExpressionSet' dt
      s.Get '/Objects/MyExpressionSet'
────────────────────────────────
 ┌Name───────┐  ┌Expression───┐
 ↓California │  ↓State in 'CA'│
 │ZeroBalance│  │Balance ==0  │
 └Char(11)───┘  └Char(13)─────┘
── 2 rows by 2 columns ─────────
~~~


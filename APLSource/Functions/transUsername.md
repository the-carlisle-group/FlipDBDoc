# transUsername

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=transUsername X
~~~

X must be the system column TWID, or a virtual column pointing to the TWID column of another table.
R is the username associated with the corresponding transaction id number. R is type Char.

See also: →[transDateTime]

## Samples

~~~
      d=Scripting.Server.OpenDatabase 'SandP'
      t=d.GetTable 'S'
      t.Evaluate 'transUsername TWID'
┌─────────┐
↓admin    │
│admin    │
│admin    │
│admin    │
│admin    │
└Char(128)┘
~~~


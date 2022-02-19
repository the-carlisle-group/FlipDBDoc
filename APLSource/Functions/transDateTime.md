# transDateTime

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

~~~
R=transDateTime X
~~~

X must be the system column TWID, or a virtual column pointing to the TWID column of another table.
R is the DateTime associated with the corresponding transaction id number.

See also: →[transUsername]

## Samples

~~~
      d=Scripting.Server.OpenDatabase 'SandP'
      t=d.GetTable 'S'
      t.Evaluate 'transDateTime TWID'
┌───────────────────┐
↓2012-10-10 09:07:31│
│2012-10-10 09:07:31│
│2012-10-10 09:07:31│
│2012-10-10 09:07:31│
│2012-10-10 09:07:31│
└DateTime───────────┘
~~~


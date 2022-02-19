# CC Property

Applies to:{.prefix}

→[##.##.Mail]{.info}

This property specifies the email addresses recipients to be copied on the email. It may be
specified as a scalar Char or as simple Char column. Regardless of how it is specified, it is
always returned as a simple Char column containing zero or more rows. For example:

~~~
      M=Scripting.Mail.New ''
      M.CC='paul@carlislegroup.com'
      M.CC
┌──────────────────────┐
↓paul@carlislegroup.com│
└Char(22)──────────────┘
      M.CC='paul@carlislegroup.com,bob@carlislegroup.com'
      M.CC
┌──────────────────────┐
↓paul@carlislegroup.com│
│bob@carlislegroup.com │
└Char(22)──────────────┘
~~~


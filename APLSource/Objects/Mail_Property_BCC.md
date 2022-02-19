# BCC Property

Applies to:{.prefix}

→[##.##.Mail]{.info}

This property specifies the email addresses of the recipients to receive a blind carbon copy. It
may be specified as a scalar Char or as simple Char column. Regardless of how it is specified, it
is always returned as a simple Char column containing zero or more rows. For example:

~~~
      M=Scripting.Mail.New ''
      M.BCC='paul@carlislegroup.com'
      M.BCC
┌──────────────────────┐
↓paul@carlislegroup.com│
└Char(22)──────────────┘
      M.BCC='paul@carlislegroup.com,bob@carlislegroup.com'
      M.BCC
┌──────────────────────┐
↓paul@carlislegroup.com│
│bob@carlislegroup.com │
└Char(22)──────────────┘
~~~


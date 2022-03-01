# Body Property

Applies to:{.prefix}

→[##.##.Mail]{.info}

This property specifies the body of the email. It must be a scalar Char. For example:

~~~
      M=Scripting.Mail.New ''
      M.Body='This is the body of the email'
~~~

If the text is HTML, then the email is format will HTML. HTML text may be generated from a Report
using the HTMLText property. For example:

~~~
      R=Scripting.Report.New ''
      R.Add 'Heading1' 'This is the in a large font.'
      R.Add 'This is in a normal font'
      M.Body=R.HTMLText
~~~


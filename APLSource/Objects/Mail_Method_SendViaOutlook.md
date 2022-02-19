# SendViaOutlook Method

Applies to:{.prefix}

→[##.##.Mail]{.info}

This method sends the email via Microsoft Outlook.

The argument is composed of 1 item:

|-|-|
|1|Dummy|Void|

The result is composed of 1 item:

|-|-|
|1|Dummy|Boolean|

For example:

~~~
      M=Scripting.Mail.New ''
      M.To='paul@carlislegroup.com'
      M.Subject='This is a test.'
      M.Body='This is the body of the email.'
      M.SendViaOutlook 0
┌───────┐
│0      │
└Boolean┘
~~~


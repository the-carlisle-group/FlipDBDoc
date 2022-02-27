# Mail Object

## Purpose

Send Email.

## Description

The Mail object may be used to create and send an email via Microsoft Outlook.
For example:

~~~
      M=Scripting.Mail.New ''
      M.To='paul@carlislegroup.com''
      M.Subject='This is a test.'
      M.Body='This is the body of the email.'
      M.SendViaOutlook 0
┌───────┐
│0      │
└Boolean┘
~~~


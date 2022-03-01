# CreateFolder Method

Applies to:{.prefix}

→[##.##.FileSystem]{.info}

This method creates a folder.

The argument is composed of 1 item:

|-|-|
|1|FolderName|String|

The result is composed of 1 item:

|-|-|
|1|FolderExists|Boolean|

If the folder does not exist, it is created and the method returns a 0.If the folder already
exists, the method does nothing and returns a 1.If the method fails for any reason, an error is signalled.


# CopyFolder Method

Applies to:{.prefix}

→[##.##.FileSystem]{.info}

This method copies a folder and all of its contents, including subfolders and their contents.

The argument is composed of 2 items:

|-|-|
|1|SourceFolderName|String|
| 2|TargetFolderName|String|

The result is composed of 1 item:

|-|-|
|1|Dummy|Boolean|

If the copy is successful, the method completes and returns a zero. If the copy is unsuccessful for
any reason, an error is signalled.


# RenameFolder Method

Applies to:{.prefix}

→[##.##.FileSystem]{.info}

This method renames or moves a folder to a new location.

The argument is composed of 2 items:

|-|-|
|1|SourceFolderName|String|
| 2|TargetFolderName|String|

The result is composed of 1 item:

|-|-|
|1|Dummy|Boolean|

If the rename is successful, the method completes and returns a zero.If the rename is unsuccessful
for any reason, an error is signalled.


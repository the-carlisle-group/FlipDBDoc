# RenameFile Method

Applies to:{.prefix}

→[##.##.FileSystem]{.info}

This method renames or moves a file to a new location.

The argument is composed of 2 items:

|-|-|
|1|SourceFileName|String|
| 2|TargetFileName|String|

The result is composed of 1 item:

|-|-|
|1|Dummy|Boolean|

If the rename is successful, the method completes and returns a zero.If the rename is unsuccessful
for any reason, an error is signalled.


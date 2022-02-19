# Locked Property

Applies to:{.prefix}

→[##.##.Database]{.info}

Specifies whether or not a Database is locked.  May be set to a Boolean scalar 1 or 0.

A server will not accept commands by a non-administrator to a locked database. The Locked property
is only relevant when connecting via TCP/IP, and has no effect in stand-alone or embedded mode.


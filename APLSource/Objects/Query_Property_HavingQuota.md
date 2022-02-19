# HavingQuota Property

Applies to:{.prefix}

→[##.##.Query]{.info}

The HavingQuota property specifies an integer that caps the number of rows identified by the
→[Having] property. The default value is 0, which indicates that no quota is in effect.

A positive value M indicates that at most M rows should be selected, starting from the top of  the
result.   A negative value N will select at most N rows, starting from the bottom of the result.

To limit the number of rows initially selected in the query, see the →[WhereQuota] property


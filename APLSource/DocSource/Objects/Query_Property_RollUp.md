# RollUp Property

Applies to:{.prefix}

→[##.##.Query]{.info}

The RollUp property specifies how groups that may be discarded due to the →[Having] and
→[HavingQuota] properties are handled.  The RollUp property only effects grouped queries.

The effect of the Having and HavingQuota properties is to discard groups from the result set of a
query.  If RollUp is 0, the default, these groups  are simply discarded. If RollUp is 1, then
these groups are combined into one new group, which is appended to the end of the result set.

Note that RollUp property will NOT select rows excluded by a non-exhaustive statement set. To
accomplish this use the key word [StatementSetComplement] as a statement.


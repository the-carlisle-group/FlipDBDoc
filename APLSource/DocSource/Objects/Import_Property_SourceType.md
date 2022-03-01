# SourceType Property

Applies to:{.prefix}

→[##.##.Import]{.info}

This property specifies the type of the data source. It may be set to "CSV", "Excel",  "CAS",
"dBase", "FixedWidth", or "ADO". Other properties of the Import object will be relevant or not
depending on the value of this property. For example, the Delimiter property is relevant if the
SourceType is "CSV". while the FixedWidthLayout and RecordLength properties are relevant if the
SourceType is "FixedWidth".


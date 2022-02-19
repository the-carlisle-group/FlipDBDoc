# COBOL Files

COBOL files are also fixed width files, but are in EBCDIC rather than ASCII. The initial import
process is the same as for ASCII Fixed Width files, requires a column name, a starting byte
position and a width in bytes. If you are lucky, you have been provided a layout that includes the
starting position and possibly the width of each field in the file, If not, you must compute it by
inspecting the COBOL copybook and keeping a running total of the  byte widths of each field as you
move through the layout. It is best to use a spreadsheet to do this.

All fields are initially imported as Char columns. However, they will be inscrutable because they
are not in ASCII or Unicode. FlipDB supports 7 COBOL field types and has 7 associated functions to
convert them to the appropriate FlipDB type. These are summarized in the following table:

|COBOL Type|Notes|
|-|-|
|Char|An EBCDIC character field. May be any number of bytes. Copybook picture looks like XX for 2 bytes X(9) for 9 bytes. Requires the function cobolChar to convert to ASCII/Unicode.|
|COMP|Also known as BINARY. May be any number of bytes. Occasionally seen in business. Has been known to contain dates. Requires the function cobolComp to convert to a FlipDB Integer or Decimal type.|
|COMP-1|A four-byte, 32-bit floating-point computational field. Rarely seen in the business world. Requires the function cobolComp1 to convert to a FlipDB Float.|
|COMP-2|An eight-byte, 32-bit floating-point computational field. Rarely seen in the business world. Requires the function cobolComp2 to convert to a FlipDB Float.|
|COMP-3|Also known as Packed or Packed Decimal. A signed computational field. This is the most commonly occurring computational field in the business world. Requires the function cobolComp3 to convert to Integer or Decimal.|
|COMP-6|Similar to COMP-3, but unsigned. Occurs occasionally in the business world. Requires the cobolComp6 function to convert to Integer or Decimal.|
|Signed|Also known as Zoned, or Zoned Decimal. Looks like a Char column of digits 0 to 9, except the last byte is packed with a digit and a sign, and thus appears as a funny looking character. May be any number of bytes. Requires function compSigned to convert to FlipDB Integer or Decimal type.|

The →[*.cobolChar] function is used for character fields, and simply converts EBCDIC to ASCII. For
example: It is often the case that a COBOL file will use a character field to store numerical
data. These will need additional functions applied for example:

The cobolComp3 function is applied to COMP-3 fields, also known as Packed or Packed Decimal fields.
In this field type, two digits are packed into a single byte, with the lowest byte containing a
digit and a sign (negative or positive). These fields will often have an implied decimal which may
be provided as the left argument to cobolComp3.


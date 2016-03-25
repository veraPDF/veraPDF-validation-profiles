# PDF/A-1A validation profile
## Rule 6.1.2-1

### Description

The % character of the file header shall occur at byte offset 0 of the file. 
			The first line of a PDF file is a header identifying the version of the PDF specification to which the file conforms

### Error details

File header does not start at byte offset 0 or does not correctly identify the version of the PDF document

* Object type: `CosDocument`
* Test condition: `headerOffset == 0 && /%PDF-\d\.\d/.test(header)`
* Specification: ISO_19005_1

## Rule 6.1.2-2

### Description

The file header line shall be immediately followed by a comment consisting of a % character followed by at 
			least four characters, each of whose encoded byte values shall have a decimal value greater than 127

### Error details

Binary comment in the file header is missing or does not comply PDF/A requirements

* Object type: `CosDocument`
* Test condition: `headerByte1 > 127 && headerByte2 > 127 && headerByte3 > 127 && headerByte4 > 127`
* Specification: ISO_19005_1

## Rule 6.1.3-1

### Description

The file trailer dictionary shall contain the ID keyword. The file trailer referred to is either the last trailer dictionary in a PDF file,
			as described in PDF Reference 3.4.4 and 3.4.5, or the first page trailer in a linearized PDF file, as described in PDF Reference F.2

### Error details

Missing ID in the document trailer

* Object type: `CosDocument`
* Test condition: `(isLinearized == true && firstPageID != null) || ((isLinearized != true) && lastID != null)`
* Specification: ISO_19005_1

## Rule 6.1.3-2

### Description

The keyword Encrypt shall not be used in the trailer dictionary

### Error details

Encrypt keyword is present in the trailer dictionary

* Object type: `CosTrailer`
* Test condition: `isEncrypted != true`
* Specification: ISO_19005_1

## Rule 6.1.3-3

### Description

No data shall follow the last end-of-file marker except a single optional end-of-line marker.

### Error details

Data is present after the last end-of-file marker

* Object type: `CosDocument`
* Test condition: `postEOFDataSize == 0`
* Specification: ISO_19005_1

## Rule 6.1.3-4

### Description

In a linearized PDF, if the ID keyword is present in both the first page trailer dictionary 
			and the last trailer dictionary, the value to both instances of the ID keyword shall be identical

### Error details

Last ID is present in the Linearized PDF and does not match the first page ID

* Object type: `CosDocument`
* Test condition: `(isLinearized != true)|| lastID == null || (firstPageID == lastID)`
* Specification: ISO_19005_1

## Rule 6.1.4-1

### Description

In a cross reference subsection header the starting object number and the range shall be separated by a single SPACE character (20h)

### Error details

Spacings of subsection headers in the cross reference table do not comply PDF/A specification

* Object type: `CosXRef`
* Test condition: `subsectionHeaderSpaceSeparated == true`
* Specification: ISO_19005_1

## Rule 6.1.4-2

### Description

The xref keyword and the cross reference subsection header shall be separated by a single EOL marker

### Error details

Spacings after the 'xref' keyword iin the cross reference table do not comply PDF/A specification

* Object type: `CosXRef`
* Test condition: `xrefEOLMarkersComplyPDFA`
* Specification: ISO_19005_1

## Rule 6.1.6-1

### Description

Hexadecimal strings shall contain an even number of non-white-space characters

### Error details

Hexadecimal string contains odd number of non-white-space characters

* Object type: `CosString`
* Test condition: `(isHex != true) || hexCount % 2 == 0`
* Specification: ISO_19005_1

## Rule 6.1.6-2

### Description

All non-white-space characters in hexadecimal strings shall be in the range 0 to 9, A to F or a to f

### Error details

Hexadecimal string contains non-white-space characters outside the range 0 to 9, A to F or a to f

* Object type: `CosString`
* Test condition: `(isHex != true) || containsOnlyHex`
* Specification: ISO_19005_1

## Rule 6.1.7-1

### Description

The value of the Length key specified in the stream dictionary shall match the number of bytes in the file
			following the LINE FEED character after the stream keyword and preceding the EOL marker before the endstream keyword

### Error details

Actual length of the stream does not match the value of the Length key in the Stream dictionary

* Object type: `CosStream`
* Test condition: `isLengthCorrect`
* Specification: ISO_19005_1

## Rule 6.1.7-2

### Description

The stream keyword shall be followed either by a CARRIAGE RETURN (0Dh) and LINE FEED (0Ah) character sequence
			or by a single LINE FEED character. The endstream keyword shall be preceded by an EOL marker

### Error details

Spacings of keywords 'stream' and 'endstream' do not comply PDF/A specification

* Object type: `CosStream`
* Test condition: `streamKeywordCRLFCompliant == true && endstreamKeywordEOLCompliant == true`
* Specification: ISO_19005_1

## Rule 6.1.7-3

### Description

A stream object dictionary shall not contain the F, FFilter, or FDecodeParms keys

### Error details

A stream object dictionary contains one of the F, FFilter, or FDecodeParms keys

* Object type: `CosStream`
* Test condition: `F == null && FFilter == null && FDecodeParms == null`
* Specification: ISO_19005_1

## Rule 6.1.8-1

### Description

The object number and generation number shall be separated by a single white-space character. The generation number and obj keyword 
	shall be separated by a single white-space character. The object number and endobj keyword shall each be preceded by an EOL marker. The obj and endobj
	keywords shall each be followed by an EOL marker.

### Error details

Spacings of object number and generation number or keywords 'obj' and 'endobj' do not comply PDF/A specification

* Object type: `CosIndirect`
* Test condition: `spacingCompliesPDFA`
* Specification: ISO_19005_1

## Rule 6.1.10-1

### Description

The LZWDecode filter shall not be permitted

### Error details

LZW compression is used

* Object type: `CosFilter`
* Test condition: `internalRepresentation != "LZWDecode"`
* Specification: ISO_19005_1

## Rule 6.1.10-2

### Description

The LZWDecode filter shall not be permitted

### Error details

LZW compression is used in the inline image

* Object type: `CosIIFilter`
* Test condition: `internalRepresentation != "LZWDecode" && internalRepresentation != "LZW"`
* Specification: ISO_19005_1

## Rule 6.1.11-1

### Description

A file specification dictionary, as defined in PDF 3.10.2, shall not contain the EF key

### Error details

A file specification dictionary contains the EF key

* Object type: `CosFileSpecification`
* Test condition: `EF_size == 0`
* Specification: ISO_19005_1

## Rule 6.1.11-2

### Description

A file's name dictionary, as defined in PDF Reference 3.6.3, shall not contain the EmbeddedFiles key

### Error details

The document contains embedded files (EmbeddedFiles key is present in the file's name dictionary)

* Object type: `CosDocument`
* Test condition: `EmbeddedFiles_size == 0`
* Specification: ISO_19005_1

## Rule 6.1.12-1

### Description

Largest Integer value is 2,147,483,647. Smallest integer value is -2,147,483,648

### Error details

Integer value out of range

* Object type: `CosInteger`
* Test condition: `(intValue <= 2147483647) && (intValue >= -2147483648)`
* Specification: ISO_19005_1

## Rule 6.1.12-2

### Description

Absolute real value must be less than or equal to 32767.0

### Error details

Real value out of range

* Object type: `CosReal`
* Test condition: `(realValue >= -32767.0) && (realValue <= 32767.0)`
* Specification: ISO_19005_1

## Rule 6.1.12-3

### Description

Maximum length of a string (in bytes) is 65535

### Error details

Maximum length of a String (65535) exceeded

* Object type: `CosString`
* Test condition: `value.length() < 65536`
* Specification: ISO_19005_1

## Rule 6.1.12-4

### Description

Maximum length of a name (in bytes) is 127

### Error details

Maximum length of a Name (127) exceeded

* Object type: `CosName`
* Test condition: `internalRepresentation.length() <= 127`
* Specification: ISO_19005_1

## Rule 6.1.12-5

### Description

Maximum capacity of an array (in elements) is 8191

### Error details

Maximum capacity of an array (8191) exceeded

* Object type: `CosArray`
* Test condition: `size <= 8191`
* Specification: ISO_19005_1

## Rule 6.1.12-6

### Description

Maximum capacity of a dictionary (in entries) is 4095

### Error details

Maximum capacity of a dictionary (4095) exceeded

* Object type: `CosDict`
* Test condition: `size <= 4095`
* Specification: ISO_19005_1

## Rule 6.1.12-7

### Description

Maximum number of indirect objects in a PDF file is 8,388,607

### Error details

Maximum number of indirect objects (8,388,607) in a PDF file exceeded

* Object type: `CosDocument`
* Test condition: `nrIndirects <= 8388607`
* Specification: ISO_19005_1

## Rule 6.1.12-8

### Description

Maximum depth of graphics state nesting by q and Q operators is 28

### Error details

Maximum depth of graphics state nesting (q and Q operators) is exceeded

* Object type: `Op_q_gsave`
* Test condition: `nestingLevel <= 28`
* Specification: ISO_19005_1

## Rule 6.1.12-9

### Description

Maximum number of DeviceN components is 8

### Error details

Maximum number of DeviceN components is exceeded

* Object type: `PDDeviceN`
* Test condition: `nrComponents <= 8`
* Specification: ISO_19005_1

## Rule 6.1.12-10

### Description

Maximum value of a CID (character identifier) is 65,535

### Error details

Maximum value of a CID (65,535) is exceeded

* Object type: `CIDGlyph`
* Test condition: `CID <= 65535`
* Specification: ISO_19005_1

## Rule 6.1.13-1

### Description

The document catalog dictionary shall not contain a key with the name OCProperties

### Error details

The document catalog dictionary contains the OCProperties entry

* Object type: `CosDocument`
* Test condition: `isOptionalContentPresent == false`
* Specification: ISO_19005_1

## Rule 6.2.2-1

### Description

A PDF/A-1 OutputIntent is an OutputIntent dictionary, as defined by PDF Reference 9.10.4, that is included in the file's OutputIntents 
	array and has GTS_PDFA1 as the value of its S key and a valid ICC profile stream as the value its DestOutputProfile key

### Error details

The embedded PDF/A Output Intent colour profile is either invalid or does not provide BToA information

* Object type: `ICCOutputProfile`
* Test condition: `(deviceClass == "prtr" || deviceClass == "mntr") && (colorSpace == "RGB " || colorSpace == "CMYK" || colorSpace == "GRAY") && version < 3.0`
* Specification: ISO_19005_1

## Rule 6.2.2-2

### Description

If a file's OutputIntents array contains more than one entry, then all entries that contain a DestOutputProfile
			key shall have as the value of that key the same indirect object, which shall be a valid ICC profile stream

### Error details

File's OutputIntents array contains output intent dictionaries with non-matching destination output profiles

* Object type: `PDOutputIntent`
* Test condition: `destOutputProfileIndirect == null || gOutputProfileIndirect == null || destOutputProfileIndirect == gOutputProfileIndirect`
* Specification: ISO_19005_1

## Rule 6.2.3-1

### Description

All ICCBased colour spaces shall be embedded as ICC profile streams as described in PDF Reference 4.5

### Error details

The embedded ICC profile is either invalid or does not satisfy PDF 1.4 requirements

* Object type: `ICCInputProfile`
* Test condition: `(deviceClass == "prtr" || deviceClass == "mntr" || deviceClass == "scnr" || deviceClass == "spac") && 
			(colorSpace == "RGB " || colorSpace == "CMYK" || colorSpace == "GRAY" || colorSpace == "LAB ") && version < 3.0`
* Specification: ISO_19005_1

## Rule 6.2.3-2

### Description

DeviceRGB may be used only if the file has a PDF/A-1 OutputIntent that uses an RGB colour space

### Error details

DeviceRGB colour space is used without RGB output intent profile

* Object type: `PDDeviceRGB`
* Test condition: `gOutputCS != null && gOutputCS == "RGB "`
* Specification: ISO_19005_1

## Rule 6.2.3-3

### Description

DeviceCMYK may be used only if the file has a PDF/A-1 OutputIntent that uses a CMYK colour space

### Error details

DeviceCMYK colour space is used without CMYK output intent profile

* Object type: `PDDeviceCMYK`
* Test condition: `gOutputCS != null && gOutputCS == "CMYK"`
* Specification: ISO_19005_1

## Rule 6.2.3-4

### Description

If an uncalibrated colour space is used in a file then that file shall contain a PDF/A-1 OutputIntent, as defined in 6.2.2

### Error details

DeviceGray colour space is used without output intent profile

* Object type: `PDDeviceGray`
* Test condition: `gOutputCS != null`
* Specification: ISO_19005_1

## Rule 6.2.3-5

### Description

All ICCBased colour spaces shall be embedded as ICC profile streams as described in PDF Reference 4.5.
			The number of color components in the color space described by the ICC profile data must match the number of components actually in the ICC profile. 
			As of PDF 1.4, N must be 1, 3, or 4

### Error details

The N entry in the ICC profile dictionary is missing or does not match the number of components in the embedded ICC profile

* Object type: `ICCProfile`
* Test condition: `N != null && ((N == 1 && colorSpace == "GRAY") || (N == 3 && (colorSpace == "RGB " || colorSpace == "LAB ")) || (N == 4 && colorSpace == "CMYK"))`
* Specification: ISO_19005_1

## Rule 6.2.4-1

### Description

An Image dictionary shall not contain the Alternates key

### Error details

Alternates key is present in the Image dictionary(

* Object type: `PDXImage`
* Test condition: `Alternates_size == 0`
* Specification: ISO_19005_1

## Rule 6.2.4-2

### Description

An XObject dictionary (Image or Form) shall not contain the OPI key

### Error details

OPI key is present in the XObject dictionary(

* Object type: `PDXObject`
* Test condition: `OPI_size == 0`
* Specification: ISO_19005_1

## Rule 6.2.4-3

### Description

If an Image dictionary contains the Interpolate key, its value shall be false

### Error details

The value of the Interpolate key in the Image dictionary is true

* Object type: `PDXImage`
* Test condition: `Interpolate == false`
* Specification: ISO_19005_1

## Rule 6.2.5-1

### Description

A form XObject dictionary shall not contain the Subtype2 key with a value of PS or the PS key

### Error details

The form XObject dictionary contains a PS key or Subtype2 key with value PS

* Object type: `PDXForm`
* Test condition: `(Subtype2 == null || Subtype2 != "PS") && PS_size == 0`
* Specification: ISO_19005_1

## Rule 6.2.6-1

### Description

A conforming file shall not contain any reference XObjects

### Error details

The document contains a reference XObject (Ref key in the form XObject dictionary)

* Object type: `PDXForm`
* Test condition: `Ref_size == 0`
* Specification: ISO_19005_1

## Rule 6.2.7-1

### Description

A conforming file shall not contain any PostScript XObjects

### Error details

The document contains a PostScript XObject

* Object type: `PDXObject`
* Test condition: `Subtype != "PS"`
* Specification: ISO_19005_1

## Rule 6.2.8-1

### Description

An ExtGState dictionary shall not contain the TR key

### Error details

An ExtGState dictionary contains the TR key

* Object type: `PDExtGState`
* Test condition: `TR == null`
* Specification: ISO_19005_1

## Rule 6.2.8-2

### Description

An ExtGState dictionary shall not contain the TR2 key with a value other than Default

### Error details

An ExtGState dictionary contains the TR2 key with a value other than Default

* Object type: `PDExtGState`
* Test condition: `TR2 == null || TR2 == "Default"`
* Specification: ISO_19005_1

## Rule 6.2.9-1

### Description

Where a rendering intent is specified, its value shall be one of the four values defined in PDF Reference
			RelativeColorimetric, AbsoluteColorimetric, Perceptual or Saturation

### Error details

A rendering intent with non-standard value is used

* Object type: `CosRenderingIntent`
* Test condition: `internalRepresentation == "RelativeColorimetric" || internalRepresentation == "AbsoluteColorimetric" || internalRepresentation == "Perceptual" || internalRepresentation == "Saturation"`
* Specification: ISO_19005_1

## Rule 6.2.10-1

### Description

A content stream shall not contain any operators not defined in PDF Reference
			even if such operators are bracketed by the BX/EX compatibility operators

### Error details

A content stream contains an operator not defined in PDF Reference

* Object type: `Op_Undefined`
* Test condition: `false`
* Specification: ISO_19005_1

## Rule 6.3.2-1

### Description

All fonts used in a conforming file shall conform to the font specifications defined in PDF Reference 5.5. 
	Type - name - (Required) The type of PDF object that this dictionary describes; must be Font for a font dictionary

### Error details

A Font dictionary has missing or invalid Type entry

* Object type: `PDFont`
* Test condition: `Type == "Font"`
* Specification: ISO_19005_1

## Rule 6.3.2-2

### Description

All fonts used in a conforming file shall conform to the font specifications defined in PDF Reference 5.5. 
			Subtype - name - (Required) The type of font; must be "Type1" for Type 1 fonts, "MMType1" for multiple master fonts, "TrueType" for TrueType fonts
			"Type3" for Type 3 fonts, "Type0" for Type 0 fonts and "CIDFontType0" or "CIDFontType2" for CID fonts

### Error details

A Font dictionary has missing or invalid Subtype entry

* Object type: `PDFont`
* Test condition: `Subtype == "Type1" || Subtype == "MMType1" || Subtype == "TrueType" || Subtype == "Type3" || Subtype == "Type0" 
			|| Subtype == "CIDFontType0" || Subtype == "CIDFontType2"`
* Specification: ISO_19005_1

## Rule 6.3.2-3

### Description

All fonts used in a conforming file shall conform to the font specifications defined in PDF Reference 5.5. 
			BaseFont - name - (Required) The PostScript name of the font

### Error details

A BaseFont entry is missing or has invalid type

* Object type: `PDFont`
* Test condition: `Subtype == "Type3" || fontName != null`
* Specification: ISO_19005_1

## Rule 6.3.2-4

### Description

All fonts used in a conforming file shall conform to the font specifications defined in PDF Reference 5.5. 
			FirstChar - integer - (Required except for the standard 14 fonts) The first character code defined in the font's Widths array

### Error details

A non-standard simple font dictionary has missing or invalid FirstChar entry

* Object type: `PDSimpleFont`
* Test condition: `isStandard == true || FirstChar != null`
* Specification: ISO_19005_1

## Rule 6.3.2-5

### Description

All fonts used in a conforming file shall conform to the font specifications defined in PDF Reference 5.5. 
			FirstChar - integer - (Required except for the standard 14 fonts) The first character code defined in the font's Widths array

### Error details

A non-standard simple font dictionary has missing or invalid LastChar entry

* Object type: `PDSimpleFont`
* Test condition: `isStandard == true || LastChar != null`
* Specification: ISO_19005_1

## Rule 6.3.2-6

### Description

All fonts used in a conforming file shall conform to the font specifications defined in PDF Reference 5.5. 
			Widths - array - (Required except for the standard 14 fonts; indirect reference preferred) An array of (LastChar − FirstChar + 1) widths

### Error details

Widths array is missing or has invalid size

* Object type: `PDSimpleFont`
* Test condition: `isStandard == true || (Widths_size != null && Widths_size == LastChar - FirstChar + 1)`
* Specification: ISO_19005_1

## Rule 6.3.3-1

### Description

For any given composite (Type 0) font referenced within a conforming file, the CIDSystemInfo entries of its 
			CIDFont and CMap dictionaries shall be compatible

### Error details

Registry and Ordering entries in the CIDFont and CMap dictionaries of a Type 0 font are not compatible

* Object type: `PDType0Font`
* Test condition: `areRegistryOrderingCompatible == true`
* Specification: ISO_19005_1

## Rule 6.3.3-2

### Description

For all Type 2 CIDFonts, the CIDFont dictionary shall contain a CIDToGIDMap entry that shall be a stream mapping 
			from CIDs to glyph indices or the name Identity, as described in PDF Reference Table 5.13

### Error details

A Type 2 CIDFont dictionary has missing or invalid CIDToGIDMap entry

* Object type: `PDCIDFont`
* Test condition: `Subtype != "CIDFontType2" || CIDToGIDMap != null`
* Specification: ISO_19005_1

## Rule 6.3.3-3

### Description

All CMaps used within a conforming file, except Identity-H and Identity-V, shall be embedded in that file as described in PDF Reference 5.6.4

### Error details

A CMap is different from "Identity-H" or "Identity-V" and is not embedded

* Object type: `PDCMap`
* Test condition: `CMapName == "Identity-H" || CMapName == "Identity-V" || embeddedFile_size == 1`
* Specification: ISO_19005_1

## Rule 6.3.3-4

### Description

For those CMaps that are embedded, the integer value of the WMode entry in the CMap dictionary shall be identical to the WMode value in the embedded CMap stream

### Error details

WMode entry in the embedded CMap and in the CMap dictionary are not identical

* Object type: `CMapFile`
* Test condition: `WMode == dictWMode`
* Specification: ISO_19005_1

## Rule 6.3.4-1

### Description

The font programs for all fonts used within a conforming file shall be embedded within that file, as defined in PDF Reference 5.8, 
			except when the fonts are used exclusively with text rendering mode 3

### Error details

The font program is not embedded

* Object type: `PDFont`
* Test condition: `Subtype == "Type3" || Subtype == "Type0" || fontFile_size == 1`
* Specification: ISO_19005_1

## Rule 6.3.5-1

### Description

Embedded font programs shall define all font glyphs referenced for rendering with a conforming file

### Error details

Not all glyphs referenced for rendering are present in the embedded font program

* Object type: `Glyph`
* Test condition: `isGlyphPresent == true`
* Specification: ISO_19005_1

## Rule 6.3.5-2

### Description

For all Type 1 font subsets referenced within a conforming file, the font descriptor dictionary shall include a CharSet string
			listing the character names defined in the font subset, as described in PDF Reference Table 5.18

### Error details

A Type1 font subset does not define CharSet entry in its Descriptor dictionary

* Object type: `PDType1Font`
* Test condition: `fontName.search(/[A-Z]{6}\+/) != 0 || CharSet != null`
* Specification: ISO_19005_1

## Rule 6.3.5-3

### Description

For all CIDFont subsets referenced within a conforming file, the font descriptor dictionary shall include a
			CIDSet stream identifying which CIDs are present in the embedded CIDFont file, as described in PDF Reference Table 5.20

### Error details

A CID Font subset does not define CIDSet entry in its Descriptor dictionary

* Object type: `PDCIDFont`
* Test condition: `fontName.search(/[A-Z]{6}\+/) != 0 || CIDSet_size == 1`
* Specification: ISO_19005_1

## Rule 6.3.6-1

### Description

For every font embedded in a conforming file, the glyph width information stored in the Widths entry of the 
			font dictionary and in the embedded font program shall be consistent

### Error details

Glyph width information in the embedded font program is not consistent with the Widths entry of the font dictionary

* Object type: `Glyph`
* Test condition: `isWidthConsistent == true`
* Specification: ISO_19005_1

## Rule 6.3.7-1

### Description

All non-symbolic TrueType fonts shall specify MacRomanEncoding or WinAnsiEncoding, either as the value of the Encoding entry in the font dictionary
			or as the value of the BaseEncoding entry in the dictionary that is the value of the Encoding entry in the font dictionary.
			If the value of the Encoding entry is a dictionary, it shall not contain a Differences entry.

### Error details

A non-symbolic TrueType font has an encoding different from MacRomanEncoding or WinAnsiEncoding

* Object type: `PDTrueTypeFont`
* Test condition: `isSymbolic == true || (Encoding == "MacRomanEncoding" || Encoding == "WinAnsiEncoding")`
* Specification: ISO_19005_1

## Rule 6.3.7-2

### Description

All symbolic TrueType fonts shall not specify an Encoding entry in the font dictionary

### Error details

A symbolic TrueType font specifies an Encoding entry in its dictionary

* Object type: `PDTrueTypeFont`
* Test condition: `isSymbolic == false || Encoding == null`
* Specification: ISO_19005_1

## Rule 6.3.7-3

### Description

Font programs' "cmap" tables for all symbolic TrueType fonts shall contain exactly one encoding

### Error details

The embedded font program for a symbolic TrueType font contains more than one cmap subtable

* Object type: `TrueTypeFontProgram`
* Test condition: `isSymbolic == false || nrCmaps == 1`
* Specification: ISO_19005_1

## Rule 6.4-1

### Description

If an SMask key appears in an ExtGState dictionary, its value shall be None

### Error details

An ExtGState contains SMask key with a value other than None

* Object type: `PDExtGState`
* Test condition: `SMask == null || SMask == "None"`
* Specification: ISO_19005_1

## Rule 6.4-2

### Description

An XObject dictionary shall not contain the SMask key

### Error details

An XObject contains an SMask key

* Object type: `PDXObject`
* Test condition: `SMask_size == 0`
* Specification: ISO_19005_1

## Rule 6.4-3

### Description

A Group object with an S key with a value of Transparency shall not be included in a form XObject. 
			A Group object with an S key with a value of Transparency shall not be included in a page dictionary

### Error details

A transparency group is present in a form XObject or page dictionary

* Object type: `PDGroup`
* Test condition: `S != "Transparency"`
* Specification: ISO_19005_1

## Rule 6.4-4

### Description

The following keys, if present in an ExtGState object, shall have the values shown: BM - Normal or Compatible

### Error details

An ExtGState dictionary contains the BM key (blend mode) with a value other than Normal or Compatible

* Object type: `PDExtGState`
* Test condition: `BM == null || BM == "Normal" || BM == "Compatible"`
* Specification: ISO_19005_1

## Rule 6.4-5

### Description

The following keys, if present in an ExtGState object, shall have the values shown: CA - 1.0

### Error details

An ExtGState dictionary contains the CA key (stroke alpha) with a value other than 1.0

* Object type: `PDExtGState`
* Test condition: `CA == null || CA - 1.0 < 0.000001 && CA - 1.0 > -0.000001`
* Specification: ISO_19005_1

## Rule 6.4-6

### Description

The following keys, if present in an ExtGState object, shall have the values shown: ca - 1.0

### Error details

An ExtGState dictionary contains the ca key (fill alpha) with a value other than 1.0

* Object type: `PDExtGState`
* Test condition: `ca == null || ca - 1.0 < 0.000001 && ca - 1.0 > -0.000001`
* Specification: ISO_19005_1

## Rule 6.5.2-1

### Description

Annotation types not defined in PDF Reference shall not be permitted. Additionally, the FileAttachment, 
			Sound and Movie types shall not be permitted

### Error details

Unknown or not permitted annotation type

* Object type: `PDAnnot`
* Test condition: `Subtype == "Text" || Subtype == "Link" || Subtype == "FreeText" || Subtype == "Line" || 
			Subtype == "Square" || Subtype == "Circle" || Subtype == "Highlight" || Subtype == "Underline" ||
			Subtype == "Squiggly" || Subtype == "StrikeOut" || Subtype == "Stamp" || Subtype == "Ink" ||
			Subtype == "Popup" || Subtype == "Widget" || Subtype == "PrinterMark" || Subtype == "TrapNet"`
* Specification: ISO_19005_1

## Rule 6.5.3-1

### Description

An annotation dictionary shall not contain the CA key with a value other than 1.0

### Error details

An annotation dictionary contains the CA key with value other than 1.0

* Object type: `PDAnnot`
* Test condition: `CA == null || CA == 1.0`
* Specification: ISO_19005_1

## Rule 6.5.3-2

### Description

An annotation dictionary shall contain the F key. The F key’s Print flag bit shall be set to 1 and its Hidden,
			Invisible and NoView flag bits shall be set to 0

### Error details

Annotation flags are either missing or have forbidden values

* Object type: `PDAnnot`
* Test condition: `F != null && (F & 4) == 4 && (F & 1) == 0 && (F & 2) == 0 && (F & 32) == 0`
* Specification: ISO_19005_1

## Rule 6.5.3-3

### Description

An annotation dictionary shall not contain the C array or the IC array unless the colour space of the
			DestOutputProfile in the PDF/A-1 OutputIntent dictionary, defined in 6.2.2, is RGB

### Error details

Annotation's color or interior color is used without specifying RGB-based destination output profile

* Object type: `PDAnnot`
* Test condition: `(C_size == 0 && IC_size == 0) || gOutputCS == "RGB "`
* Specification: ISO_19005_1

## Rule 6.5.3-4

### Description

For all annotation dictionaries containing an AP key, the appearance dictionary that it defines as its value shall contain only the N key. 
			If an annotation dictionary's Subtype key has a value of Widget and its FT key has a value of Btn, the value of the N key shall be an appearance 
			subdictionary; otherwise the value of the N key shall be an appearance stream.

### Error details

Annotation's appearance dictionary contains entries other than N or the N entry has an invalid type

* Object type: `PDAnnot`
* Test condition: `AP == null || ( AP == "N" && ( ((Subtype != "Widget" || FT != "Btn") && N_type == "Stream")
			|| (Subtype == "Widget" && FT == "Btn" && N_type == "Dict") ) )`
* Specification: ISO_19005_1

## Rule 6.6.1-1

### Description

The Launch, Sound, Movie, ResetForm, ImportData and JavaScript actions shall not be permitted. 
			Additionally, the deprecated set-state and no-op actions shall not be permitted. The Hide action shall not be permitted (Corrigendum 2)

### Error details

Unknown or not permitted action type

* Object type: `PDAction`
* Test condition: `S == "GoTo" || S == "GoToR" || S == "Thread" || S == "URI" || S == "Named" || S == "SubmitForm"`
* Specification: ISO_19005_1

## Rule 6.6.1-2

### Description

Named actions other than NextPage, PrevPage, FirstPage, and LastPage shall not be permitted

### Error details

Unknown or not permitted action type

* Object type: `PDNamedAction`
* Test condition: `N == "NextPage" || N == "PrevPage" || N == "FirstPage" || N == "LastPage"`
* Specification: ISO_19005_1

## Rule 6.6.1-3

### Description

Interactive form fields shall not perform actions of any type

### Error details

Interactive form field contains an action object ('A' key)

* Object type: `PDAnnot`
* Test condition: `Subtype != "Widget" || A_size == 0`
* Specification: ISO_19005_1

## Rule 6.6.2-1

### Description

A Widget annotation dictionary shall not include an AA entry for an additional-actions dictionary

### Error details

A Widget annotation contains an additional-actions dictionary (AA entry)

* Object type: `PDAnnot`
* Test condition: `Subtype != "Widget" || AA_size == 0`
* Specification: ISO_19005_1

## Rule 6.6.2-2

### Description

A Field dictionary shall not include an AA entry for an additional-actions dictionary

### Error details

A Field dictionary contains an additional-actions dictionary (AA entry)

* Object type: `PDFormField`
* Test condition: `AA_size == 0`
* Specification: ISO_19005_1

## Rule 6.6.2-3

### Description

The document catalog dictionary shall not include an AA entry for an additional-actions dictionary

### Error details

The document catalog dictionary contains an additional-actions dictionary (AA entry)

* Object type: `PDDocument`
* Test condition: `AA_size == 0`
* Specification: ISO_19005_1

## Rule 6.7.2-1

### Description

The document catalog dictionary of a conforming file shall contain the Metadata key.

### Error details

The document catalog dictionary doesn't contain metadata key.

* Object type: `PDDocument`
* Test condition: `metadata_size == 1`
* Specification: ISO_19005_1

## Rule 6.7.2-2

### Description

Metadata object stream dictionaries shall not contain the Filter key.

### Error details

The metadata object stream dictionary contains the Filter key.

* Object type: `PDMetadata`
* Test condition: `Filter == null`
* Specification: ISO_19005_1

## Rule 6.7.3-1

### Description

If a document information dictionary does appear at a document, then all of its entries that have analogous properties in predefined XMP schemas, shall also be embedded in the file in XMP form with equivalent values.

### Error details

Some of document information dictionary entries' that have analogous properties in predefined XMP schemas do not embedded or have not equivalent values in the file in XMP form.

* Object type: `CosDocument`
* Test condition: `doesInfoMatchXMP`
* Specification: ISO_19005_1

## Rule 6.7.5-1

### Description

The bytes attribute shall not be used in the header of an XMP packet.

### Error details

The XMP Package contains bytes attribute.

* Object type: `XMPPackage`
* Test condition: `bytes == null`
* Specification: ISO_19005_1

## Rule 6.7.5-2

### Description

The encoding attribute shall not be used in the header of an XMP packet.

### Error details

The XMP Package contains encoding attribute.

* Object type: `XMPPackage`
* Test condition: `encoding == null`
* Specification: ISO_19005_1

## Rule 6.7.8-1

### Description

Extension schema descriptions shall be specified using the PDF/A extension schema description schema defined in this clause.

### Error details

An extension schema object contains fields not defined by the specification

* Object type: `ExtensionSchemaObject`
* Test condition: `containsUndefinedFields == false`
* Specification: ISO_19005_1

## Rule 6.7.8-2

### Description

The extension schema container schema uses the namespace URI "http://www.aiim.org/pdfa/ns/extension/". 
			The required schema namespace prefix is pdfaExtension. pdfaExtension:schemas - Bag Schema - Description of extension schemas

### Error details

Invalid syntax of the extension schema container

* Object type: `ExtensionSchemasContainer`
* Test condition: `isValidBag == true && prefix == "pdfaExtension"`
* Specification: ISO_19005_1

## Rule 6.7.8-3

### Description

The Schema type is an XMP structure containing the definition of an extension schema. The field namespace URI is "http://www.aiim.org/pdfa/ns/schema#". 
			The required field namespace prefix is pdfaSchema. The Schema type includes the following fields: pdfaSchema:schema (Text), pdfaSchema:namespaceURI (URI), pdfaSchema:prefix (Text), 
			pdfaSchema:property (Seq Property), pdfaSchema:valueType (Seq ValueType).

### Error details

Invalid Extension Schema definition

* Object type: `ExtensionSchemaDefinition`
* Test condition: `(isSchemaValidText == true && (schemaPrefix == null || schemaPrefix == "pdfaSchema") ) && 
			(isNamespaceURIValidURI == true && ( (ExtensionSchemaProperties_size == 0 && namespaceURIPrefix == null) || namespaceURIPrefix == "pdfaSchema" ) ) &&
			(isPrefixValidText == true && (prefixPrefix == null || prefixPrefix == "pdfaSchema") ) &&
			(isPropertyValidSeq == true && (propertyPrefix == null || propertyPrefix == "pdfaSchema") ) &&
			(isValueTypeValidSeq == true && (valueTypePrefix == null || valueTypePrefix == "pdfaSchema") )`
* Specification: ISO_19005_1

## Rule 6.7.8-4

### Description

The Property type defined is an XMP structure containing the definition of a schema property. The
		field namespace URI is "http://www.aiim.org/pdfa/ns/property#". The required field namespace prefix is
		pdfaProperty. The Property type includes the following fields: pdfaProperty:name (Text), pdfaProperty:valueType (Open Choice of Text), 
		pdfaProperty:category (Closed Choice of Text), pdfaProperty:description (Text).

### Error details

Invalid extension schema Property type definition

* Object type: `ExtensionSchemaProperty`
* Test condition: `(isNameValidText == true && namePrefix == "pdfaProperty" ) && 
			(isValueTypeValidText == true && isValueTypeDefined == true && valueTypePrefix == "pdfaProperty" ) &&
			(isCategoryValidText == true && (category == "external" || category == "internal") && categoryPrefix == "pdfaProperty") &&
			(isDescriptionValidText == true && descriptionPrefix == "pdfaProperty" )`
* Specification: ISO_19005_1

## Rule 6.7.8-5

### Description

The ValueType type is an XMP structure containing the definition of all property value
		types used by embedded extension schemas that are not defined in the XMP Specification. The field namespace URI is "http://www.aiim.org/pdfa/ns/type#". 
		The required field namespace prefix is pdfaType. The ValueType type includes the following fields: pdfaType:type (Text), pdfaType:namespaceURI (URI), 
		pdfaType:prefix (Text), pdfaType:description (Text), pdfaType:field (Seq Field).

### Error details

Invalid extension schema ValueType type definition

* Object type: `ExtensionSchemaValueType`
* Test condition: `(isTypeValidText == true && typePrefix == "pdfaType" ) && 
			(isNamespaceURIValidURI == true && namespaceURIPrefix == "pdfaType" ) &&
			(isPrefixValidText == true && (prefixPrefix == null || prefixPrefix == "pdfaType") ) &&
			(isDescriptionValidText == true && descriptionPrefix == "pdfaType" ) &&
			(isFieldValidSeq == true && fieldPrefix == "pdfaType")`
* Specification: ISO_19005_1

## Rule 6.7.8-6

### Description

The Field type defined in Table 6 is an XMP structure containing the definition of a property value type field. The field 
		namespace URI is "http://www.aiim.org/pdfa/ns/field#". The required field namespace prefix is pdfaField. The Field type contains the following fields:
		pdfaField:name (Text), pdfaField:valueType (Open Choice of Text), pdfaField:description (Text).

### Error details

Invalid extension schema Field type definition

* Object type: `ExtensionSchemaField`
* Test condition: `(isNameValidText == true && namePrefix == "pdfaField" ) && 
			(isValueTypeValidText == true && isValueTypeDefined == true && valueTypePrefix == "pdfaField" ) &&
			(isDescriptionValidText == true && descriptionPrefix == "pdfaField" )`
* Specification: ISO_19005_1

## Rule 6.7.9-1

### Description

The metadata stream shall conform to XMP Specification and well formed PDFAExtension Schema for all extensions

### Error details

%1

* Object type: `XMPPackage`
* Test condition: `isSerializationValid`
* Specification: ISO_19005_1

## Rule 6.7.9-2

### Description

Properties specified in XMP form shall use either the predefined schemas defined in XMP Specification, 
			or extension schemas that comply with XMP Specification

### Error details

%1

* Object type: `XMPProperty`
* Test condition: `(isPredefinedInXMP2004 == true || isDefinedInCurrentPackage == true) && isValueTypeCorrect == true`
* Specification: ISO_19005_1

## Rule 6.7.11-1

### Description

The PDF/A version and conformance level of a file shall be specified using the PDF/A Identification extension schema.

### Error details

The document metadata stream doesn't contains PDF/A Identification Schema.

* Object type: `MainXMPPackage`
* Test condition: `Identification_size == 1`
* Specification: ISO_19005_1

## Rule 6.7.11-2

### Description

The value of pdfaid:part shall be the part number of ISO 19005 to which the file conforms.

### Error details

The "part" property of the PDF/A Identification Schema is %1 instead of 1 for PDF/A-1 conforming file.

* Object type: `PDFAIdentification`
* Test condition: `part == 1`
* Specification: ISO_19005_1

## Rule 6.7.11-3

### Description

A Level A conforming file shall specify the value of pdfaid:conformance as A.

### Error details

The "conformance" property of the PDF/A Identification Schema is %1 instead of "A" for PDF/A-1a conforming file.

* Object type: `PDFAIdentification`
* Test condition: `conformance == "A"`
* Specification: ISO_19005_1

## Rule 6.7.11-4

### Description

The PDF/A Identification schema defined in Table 8 uses the namespace URI "http://www.aiim.org/pdfa/ns/id/". 
			The required schema namespace prefix is pdfaid. It contains the following fields: pdfaid:part (Open Choice of Integer), 
			pdfaid:amd (Open Choice of Text), pdfaid:conformance (Open Choice of Text)

### Error details

A property of the PDF/A Identification Schema has an invalid namespace prefix

* Object type: `PDFAIdentification`
* Test condition: `partPrefix == "pdfaid" && conformancePrefix == "pdfaid" &&
			(amdPrefix == null || amdPrefix == "pdfaid")`
* Specification: ISO_19005_1

## Rule 6.8.2-1

### Description

The document catalog dictionary shall include a MarkInfo dictionary whose sole entry, Marked, shall have a value of true

### Error details

/Marked entry of the MarkInfo dictionary is not present in the document catalog or is set to false

* Object type: `CosDocument`
* Test condition: `Marked == true`
* Specification: ISO_19005_1

## Rule 6.8.3-1

### Description

The logical structure of the conforming file shall be described by a structure hierarchy rooted in the StructTreeRoot entry of the document catalog dictionary, as described in PDF Reference 9.6

### Error details

StructTreeRoot entry is not present in the document catalog

* Object type: `PDDocument`
* Test condition: `StructTreeRoot_size == 1`
* Specification: ISO_19005_1

## Rule 6.8.3-2

### Description

Each structure element dictionary in the structure hierarchy shall have a Type entry with the name value of StructElem

### Error details

Missing or invalid Type entry in the structure element dictionary

* Object type: `PDStructElem`
* Test condition: `Type == "StructElem"`
* Specification: ISO_19005_1

## Rule 6.9-1

### Description

The NeedAppearances flag of the interactive form dictionary shall either not be present or shall be false

### Error details

The interactive form dictionary contains the NeedAppearances flag with value true

* Object type: `PDAcroForm`
* Test condition: `NeedAppearances == null || NeedAppearances == false`
* Specification: ISO_19005_1

## Rule 6.9-2

### Description

A Widget annotation dictionary or Field dictionary shall not contain the A or AA keys

### Error details

A Widget annotation contains either A or AA entry

* Object type: `PDAnnot`
* Test condition: `Subtype != "Widget" || (A_size == 0 && AA_size == 0)`
* Specification: ISO_19005_1

## Rule 6.9-3

### Description

A Widget annotation dictionary or Field dictionary shall not contain the A or AA keys

### Error details

A Form field dictionary contains the AA entry

* Object type: `PDFormField`
* Test condition: `AA_size == 0`
* Specification: ISO_19005_1


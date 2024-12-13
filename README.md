#  PDF to text extractor DLL for VBA

A fork of a fork of pdftotext from [Xpdf](http://www.xpdfreader.com/ "Xpdf"), modified by Peter Dey and compiled as a 64-bit DLL to allow for text extraction of PDF in VBA.

# Usage
```vb
#If VBA7 Then
  Private Declare PtrSafe Function getNumPages Lib "pdftotext.dll" (ByVal lpFileName As String, Optional ByVal lpLogCallbackFunc As LongPtr, Optional ByVal lpOwnerPassword As String, Optional ByVal lpUserPassword As String) As Integer
  Private Declare PtrSafe Function extractText Lib "pdftotext.dll" (ByVal lpFileName As String, ByVal lpTextOutput As LongPtr, Optional ByVal iFirstPage As Integer, Optional ByVal iLastPage As Integer, Optional ByVal lpTextOutEnc As String, Optional ByVal lpLayout As String, Optional ByVal lpLogCallbackFunc As LongPtr, Optional ByVal lpOwnerPassword As String, Optional ByVal lpUserPassword As String) As Integer
#Else
  Private Enum LongPtr
  [_]
  End Enum
  Private Declare Function getNumPages Lib "pdftotext.dll" (ByVal lpFileName As String, Optional ByVal lpLogCallbackFunc As LongPtr, Optional ByVal lpOwnerPassword As String, Optional ByVal lpUserPassword As String) As Integer
  Private Declare Function extractText Lib "pdftotext.dll" (ByVal lpFileName As String, ByVal lpTextOutput As LongPtr, Optional ByVal iFirstPage As Integer, Optional ByVal iLastPage As Integer, Optional ByVal lpTextOutEnc As String, Optional ByVal lpLayout As String, Optional ByVal lpLogCallbackFunc As LongPtr, Optional ByVal lpOwnerPassword As String, Optional ByVal lpUserPassword As String) As Integer
#End If

Sub Test()
  Dim strOutput As String, Filename As String, Ret As Integer
  Filename = "C:\VBA\TestPDF.pdf"
  Ret = extractText(Fname, VarPtr(strOutput), 1, 3, "UTF-8", "rawOrder", AddressOf LogCallback)
  Debug.Print StrConv(strOutput, vbUnicode)
End Sub

Sub Test2()
  Dim strOutput As String, Filename As String, Pages As Long, Ret As Integer
  Filename = "C:\VBA\TestPDF.pdf"
  Pages = getNumPages(Filename)
  Ret = extractText(Filename, VarPtr(strOutput), 1, Pages \ 2)
  Debug.Print StrConv(strOutput, vbUnicode)
End Sub

Public Sub LogCallback(ByVal str As String)
  Debug.Print "Log: " & str
End Sub
```

Almost all arguments are optional.  For example, the following works:
```vb
Dim strOutput as String
pages = getNumPages("filename.pdf")
ret = extractText("filename.pdf", VarPtr(strOutput))
```

## Acceptable values for lpTextOutEnc
* UTF-8 (default)
* Latin1
* ASCII7
* Symbol
* ZapfDingbats
* UCS-2

## Acceptable values for lpLayout
| lpLayout | Description |
| ------------ | ------------ |
| NULL (default) | format into reading order |
| rawOrder | keep strings in content stream order |
| table | similar to PhysLayout, but optimized for tables |
| phys | maintain original physical layout |
| simple | simple one-column physical layout |
| simple2 | simple one-column physical layout (alternative) |
| linePrinter | strict fixed-pitch/height layout |

# Building
```console
git clone https://github.com/peterdey/xpdf-dll
cd xpdf
mkdir build
cd build 
cmake ..
cd ..
cmake --build build --config release
```
Output is in `build/xpdf/Release/pdftotext.dll`

## Requirements:

* Visual Studio 2019
* cmake version 3.21.3

# License & Distribution

Xpdf, and therefore this project, is licensed under the GNU General Public License (GPL), version 2
or 3.  This means that you can distribute derivatives of Xpdf under
any of the following:
  - GPL v2 only
  - GPL v3 only
  - GPL v2 or v3

The Xpdf source package includes the text of both GPL versions:
COPYING for GPL v2, COPYING3 for GPL v3.

Please note that Xpdf is NOT licensed under "any later version" of the
GPL, as I have no idea what those versions will look like.

If you are redistributing unmodified copies of Xpdf (or any of the
Xpdf tools) in binary form, you need to include all of the
documentation: README, man pages (or help files), COPYING, and
COPYING3.

If you want to incorporate the Xpdf source code into another program
(or create a modified version of Xpdf), and you are distributing that
program, you have two options: release your program under the GPL (v2
and/or v3), or purchase a commercial Xpdf source license.

If you're interested in commercial licensing, please see the Glyph &
Cog web site:

    http://www.glyphandcog.com/

## Third-Party Libraries

Xpdf uses the following libraries:
* FreeType [http://www.freetype.org/]
* libpng [http://www.libpng.com/pub/png/libpng.html] (used by pdftohtml and pdftopng)
* zlib [http://zlib.net/] (used by pdftohtml)

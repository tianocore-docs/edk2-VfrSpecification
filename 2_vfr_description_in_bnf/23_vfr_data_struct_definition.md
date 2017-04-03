<!--- @file
  2.3 VFR Data Struct Definition

  Copyright (c) 2007-2017, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

## 2.3 VFR Data Struct Definition

```c
vfrDataStructDefinition ::=
 { "typedef" } "struct" 
 { StringIdentifier }
 "{" vfrDataStructFields "}"
 { StringIdentifier } ";"
 
vfrDataStructFields ::=
  (
      dataStructField64
    | dataStructField32
    | dataStructField16
    | dataStructField8
    | dataStructFieldBool
    | dataStructFieldString
    | dataStructFieldDate
    | dataStructFieldTime
    | dataStructFieldRef
    | dataStructFieldUser
  )*
  
dataStructField64 ::=
  "UINT64" 
  StringIdentifier { "[" Number "]" } ";"
                          
dataStructField32 ::=
  "UINT32" 
  StringIdentifier { "[" Number "]" } ";"
                          
dataStructField16 ::=
  "UINT16" 
  StringIdentifier { "[" Number "]" } ";"
                          
dataStructField8 ::= 
  "UINT8" 
  StringIdentifier { "[" Number "]" } ";"
                          
dataStructFieldBool ::= 
  "BOOLEAN" 
  StringIdentifier { "[" Number "]" } ";"
                          
dataStructFieldString ::= 
  "EFI_STRING_ID" 
  StringIdentifier { "[" Number "]" } ";"
                          
dataStructFieldDate ::= 
  "EFI_HII_DATE" 
  StringIdentifier { "[" Number "]" } ";"
                          
dataStructFieldTime ::= 
  "EFI_HII_TIME" 
  StringIdentifier { "[" Number "]" } ";"
                          
dataStructFieldRef ::= 
  "EFI_HII_REF" 
  StringIdentifier { "[" Number "]" } ";"
                          
dataStructFieldUser ::= 
  StringIdentifier 
  StringIdentifier { "[" Number "]" } ";"
```

#### BEHAVIORS AND RESTRICTIONS

The data structure definition is in C-style language. `enum` type is not
supported. The keyword of the fields' type must be a user defined data
structure or one of these types: `UINT8`, `UINT16`, `UINT32`, `UINT64`,
`BOOLEAN`, `EFI_STRING_ID`, `EFI_HII_DATA`, `EFI_HII_TIME EFI_HII_REF`, and at
most one-dimensional array is permitted.

#### Example

```c
typedef struct {
  UINT8 mU8;
  UINT16 mU16;
  UINT32 mU32[10];
  UINT64 mU64;
} MyData;
```

**Unsupported Example of enum type:**

```c
typedef enum {
  MyEnumValue1,
  MyEnumValue2,
  MyEnumValueMax
} MyEnum;
```

**Unsupported Example of two dimensional arrays in data structure:**

```c
typedef struct {
  UINT8   mU8;
  UINT32  mU32[10][20];
} MyUnsupportedData;
```

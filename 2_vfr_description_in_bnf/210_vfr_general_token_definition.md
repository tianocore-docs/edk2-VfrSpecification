<!--- @file
  2.10 VFR General Token Definition

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

## 2.10 VFR General Token Definition

### 2.10.1 GUID Definition

```c
Hex8 ::=
  "0x"["0"-"9""A"-"F""a"-"f"]{1,8}

Hex4 ::=
  "0x"["0"-"9""A"-"F""a"-"f"]{1,4}

Hex2 ::=
  "0x"["0"-"9""A"-"F""a"-"f"]{1,2}

guidSubDefinition ::=
  Hex2 "," Hex2 "," Hex2 "," Hex2 ","
  Hex2 "," Hex2 "," Hex2 "," Hex2

guidDefinition ::=
  "{"
  Hex8 "," Hex4 "," Hex4 ","
  (
      "{" guidSubDefinition "}"
    | guidSubDefinition
  )
  "}"
```

#### BEHAVIORS AND RESTRICTIONS

In practice, the VFR only supports GUIDs in a C-style language structure. It is
defined as two 32-bit values, followed by two 16-bit values, followed by eight
1-byte values.

### 2.10.2 String & String Identifier Definition

```c
StringIdentifier ::=
  ["A"-"Z""a"-"z""_"]["A"-"Z""a"-"z""_""0"-"9"]*

getStringId ::=
  "STRING_TOKEN" "(" Number ")"
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

### 2.10.3 Number Definition

```c
Number ::=
  ( "0x"["0"-"9""A"-"F""a"-"f"]+ ) | ["0"-"9"]+
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

### 2.10.4 VFR Statement Header Definition

```c
vfrStatementHeader  ::=
  "prompt" "=" getStringId ","
  "help"   "=" getStringId ","
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

### 2.10.5 VFR Question Header Definition

```c
vfrQuestionHeader ::=
  { "name" "=" StringIdentifier "," }
  { "varid" "=" vfrStorageVarId "," }
  { "questionid" "=" Number "," }
  vfrStatementHeader

questionheaderFlagsField ::=
    "READ_ONLY"
  | "INTERACTIVE"
  | "RESET_REQUIRED"
  | "OPTIONS_ONLY"

vfrStorageVarId ::=
  ( StringIdentifier "[" Number "]" )
  |
  (
    StringIdentifier
    (
      "." StringIdentifier { "[" Number "]" }
    )*
  )
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `questionid` is used to specify the question ID. If it is not defined,
the compiler automatically assigns a unique ID.
**********
**Note:** `name` is used to specify the reference name, which is optional.
**********
**Note:** The first `StringIdentifier` defined in `vfrStorageVarId` is the
varstore name or the structure name referred by varstore. When the same
structure is referred to by more than one varstore statement, only the varstore
name can be used here. If it is not defined, this question has no storage.
**********

####Example

None.

### 2.10.6 VFR Constant Value Definition

```c
vfrConstantValueField  ::=
    Number
  | "TRUE"
  | "FALSE"
  | "ONE"
  | "ONES"
  | "ZERO"
  | Number ":" Number ":" Number
  | Number "/" Number "/" Number
  | "STRING_TOKEN" "(" Number ")"
  | { Number ("," Number)* }
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

### 2.10.7 VFR Statement Image & Locked Tag Definition

```c
vfrImageTag ::=
  "image" "=" "IMAGE_TOKEN" "(" Number ")"

vfrLockedTag ::=
  "locked"

vfrStatementStatTag ::=
  vfrImageTag | vfrLockedTag

vfrStatementStatTagList ::=
  vfrStatementStatTag ( "," vfrStatementStatTag )*
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

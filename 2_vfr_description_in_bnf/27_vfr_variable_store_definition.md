<!--- @file
  2.7 VFR Variable Store Definition

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

## 2.7 VFR Variable Store Definition

### 2.7.1 VFR Buffer Store Definition

```c
vfrStatementVarStoreLinear ::=
  "varstore"
  (
      StringIdentifier ","
    | "UINT8" ","
    | "UINT16" ","
    | "UINT32" ","
    | "UINT64" ","
    | "EFI_HII_DATE" ","
    | "EFI_HII_TIME" ","
    | "EFI_HII_REF" ","
  )
  { "varid" "=" Number "," }
  "name" "=" StringIdentifier ","
  "guid" "=" guidDefinition ";"
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** The `StringIdentifier` following `varstore` is the referred data
structure name. The `StringIdentifier` of `name` is the varstore name.
**********
**Note:** `name` and `guid` are used jointly to specify the variable store.
**********

#### Example

```
varstore MyData, name = RefName, guid = FORMSET_GUID;
```

### 2.7.2 VFR EFI Variable Store Definition

```c
vfrStatementVarStoreEfi ::=
  "efivarstore"
  (
      StringIdentifier ","
    | "UINT8" ","
    | "UINT16" ","
    | "UINT32" ","
    | "UINT64" ","
    | "EFI_HII_DATE" ","
    | "EFI_HII_TIME" ","
    | "EFI_HII_REF" ","
  )
  { "varid" "=" Number "," }
  "attribute" "=" Number ( "|" Number )* ","
  "name" "=" StringIdentifier ","
  "guid" "=" guidDefinition ";"
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** The `StringIdentifier` following `efivarstore` is the referred
data structure name. The `StringIdentifier` of `name` is the varstore name.
**********
**Note:** `name` and `guid` are used jointly to specify the efi variable store.
**********

#### Example

```c
efivarstore EfiDataStructure
  attribute = EFI_VARIABLE_BOOTSERVICE_ACCESS,
  name      = EfiData,
  guid      = GUID;
```

### 2.7.3 VFR Variable Name Store Definition

```c
vfrStatementVarStoreNameValue ::=
  "namevaluevarstore" StringIdentifier ","
  { "varid" "=" Number "," }
  ( "name" "=" getStringId "," )+
  "guid" "=" guidDefinition ";"
```

#### BEHAVIORS AND RESTRICTIONS

#### Example

```c
namevaluevarstore NameValueVarStore,
  name = STRING_TOKEN(STR_NAMEVALUE_TABLE_ITEM1),
  name = STRING_TOKEN(STR_NAMEVALUE_TABLE_ITEM2),
  name = STRING_TOKEN(STR_NAMEVALUE_TABLE_ITEM3),
  guid = GUID;
```

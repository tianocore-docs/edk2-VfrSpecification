<!--- @file
  2.6 VFR Default Stores Definition

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

## 2.6 VFR Default Stores Definition

```c
vfrStatementDefaultStore ::=
  "defaultstore" StringIdentifier "," 
  "prompt" "=" getStringId
  { "," "attribute" "=" Number } ";"
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `attribute` is used to specify the default ID.
**********

It is optional. The Standard Defaults Identifier,
`EFI_HII_DEFAULT_CLASS_STANDARD`, is used if the attribute is not defined. The
Manufacturing Defaults Identifier, `EFI_HII_DEFAULT_CLASS_MANUFACTURING,` and
the Safe Defaults Identifier, `EFI_HII_DEFAULT_CLASS_SAFE,` can be also used if
the header file defining them is included.

#### Example

```c
defaultstore MyStandard, prompt = STRING_TOKEN(STR_STANDARD_DEFAULT);
```

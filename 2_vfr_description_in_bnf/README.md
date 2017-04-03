<!--- @file
  2 VFR Description in BNF

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

# 2 VFR Description in BNF

This section describes a language for developers to use when writing forms.
This language is compiled into an architectural form (i.e., IFR) described in
the _UEFI Specification_. Each section describes a different language terminal
or non-terminal.

**********
**Note:** The text formatting of the VFR statements and examples in this
document are consistent with BNF-style conventions rather than with EFI or UEFI
conventions.
**********

VFR forms consist of a series of VFR statements. The subsections in this
chapter designate the base definitions of the different VFR statements by
specifying the keyword _"Statement"_ in the section title, followed by the BNF
non-terminal name enclosed in parentheses.

Subsections without the _"Statement"_ keyword in the title are non-terminal
definitions referenced by one or more of the VFR statement definitions.

The VFR language uses BNF-style syntax:

* For the BNF-style syntax used here, literal characters and terms are in bold
  and marked with quotation mark. **Example:** "formset" is composed of literal
  characters:
  ```c
  "formset"
  ```

* Terms are case-sensitive. VFR comments start with "//" and end at the end of
  the line. **Example:** a comment line:
  ```c
  // this is a typical comment marker
  ```

* Optional terms are enclosed in non-bolded braces {}. **Example:** "classguid"
  definition is optional for formset:
  ```c
  { "classguid" "=" classguidDefinition "," }
  ```

* Terms enclosed in parentheses, "()", and followed by an asterisk, "*" may be
  specified in the input VFR zero or more times. **Example:**
  ", vfrStatementStatTag" could appear zero or more times in
  vfrStatementStatTagList:
  ```c
  vfrStatementStatTagList ::=
    vfrStatementStatTag ( "," vfrStatementStatTag )*
  ```

* If the parenthesis is followed by a plus "+" sign, then the term must be
  present one or more times. **Example:** one or more numbers could be used in
  ideqvallist expression:
  ```c
  ideqvallistExp ::=
    "ideqvallist"
    vfrQuestionDataFieldName "==" ( Number )+
  ```

* Groups of terms are sometimes enclosed in parentheses. The character "|"
  between the two terms indicates that either term has acceptable syntax.
  **Example:** either "push" or "pop" is acceptable for vfrPragmaPackType:
  ```c
  vfrPragmaPackType ::=
    {
        "show"
      | ( "push" | "pop" ) { "," StringIdentifier } { "," Number }
      | { Number }
    }
  ```

* A superscript number, "n", followed by a comma "," and another number, "m",
  such as item{n, m} is used to indicate that the number of occurrences can be
  from n to m occurrences of the item, inclusive. **Example:** there could be 1
  to 8 hexadecimal characters in "Hex8":
  ```c
  Hex8 ::= "0x"["0"-"9""A"-"F""a"-"f"]{1,8}
  ```

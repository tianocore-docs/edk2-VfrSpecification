<!--- @file
  README.md for EDK II VFR Programming Language Specification

  Copyright (c) 2007-2018, Intel Corporation. All rights reserved.<BR>

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

<img src="media/TianocoreTitlePageLogo.jpg" width="300" />

### {{ book.title }}

{% if book.draft %}
** DRAFT FOR REVIEW **
{% else %}
** {{ book.version }} **
{% endif %}

** {{ gitbook.time|date('MM/DD/YYYY hh:mm:ss') }} **

{% if book.udkrelease %}
** {{ book.udkrelease }} **
{% endif %}


### Acknowledgements

Redistribution and use in source (original document form) and 'compiled'
forms (converted to PDF, epub, HTML and other formats) with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code (original document form) must retain the
   above copyright notice, this list of conditions and the following
   disclaimer as the first lines of this file unmodified.

2. Redistributions in compiled form (transformed to other DTDs, converted to
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

Copyright (c) 2007-2018, Intel Corporation. All rights reserved.


### Revision History

| Revision | Revision History                                                                                                                                                                                               | Date              |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| 0.90     | Preliminary release                                                                                                                                                                                            | December 12, 2007 |
| 1.00     | First general release                                                                                                                                                                                          | December 21 2007  |
| 1.10     | Update some syntax and fix some literal issues                                                                                                                                                                 | February 20, 2008 |
| 1.20     | Add more defaultstore ID description and fix some literal errors                                                                                                                                               | May 26, 2008      |
| 1.30     | Add new syntax classguid, suppressifformset, security, formmap to support UEFI 2.3 HII IFR opcodes, update some syntax and fix some literal errors.                                                            | Feb 12, 2010      |
| 1.40     | Add generic Guid opcode, inconsistentif check for EFI_IFR_TIME opcode, and, support question VarStorageId defined by DataType and VarStore name.                                                               | May 3 , 2011      |
| 1.50     | Update efi varstore syntax, add modal, refresh event group and REF5 opcode, and add EFI_HII_REF data type to support UEFI 2.3.1 HII IFR opcodes, corrects Time statement example. Minor editing and formatting | August 31, 2011   |
| 1.60     | Update syntax for goto, image, questionref and constant value opcodes, correct CALLBACK flag to INTEREACTIVE, correct help string for old syntax date/time example, and add examples for expression opcodes.   | December 1, 2011  |
| 1.70     | Clarify restriction that enum type and struct data filed with more than one dimensions array are not supported.                                                                                                | May 18, 2012      |
| 1.80     | Add syntax for warningif opcode, update definition for name/value varstore and subtitle opcode, update referenced UEFI spec version info.                                                                      | Jan 14, 2014      |
| 1.90     | Correct sample code for catenate/match/cond opcode. Add syntax for match2 opcode. Add sample code to show the buffer type constant value for orderedlist opcode and default opcode.                            | July 2, 2015      |
| 1.91     | Convert to Gitbook                                                                                                                                                                                             | April 2017        |
| 1.92     | [#683](https://bugzilla.tianocore.org/show_bug.cgi?id=683) VFR Spec: Add union data type and bit fields in VFR Data Struct Definition                                                                          | Mar 2018          |


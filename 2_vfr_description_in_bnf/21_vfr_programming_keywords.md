<!--- @file
  2.1 VFR Programming Keywords

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

## 2.1 VFR Programming Keywords

The following keywords constitute the working set of commands for the VFR
language. An example line of VFR code follows each keyword description to help
programmers understand how a sample such code should look.

### // (comment marker)

This allows a programmer to leave comments in the VFR file. They have no effect
on the IFR binary that is generated.  **Example:**

```c
// this is a typical comment marker
```

### #define

This command is used to assign a meaningful name to a constant, and is very
similar in function to the 'C' style.  **Example:**

```c
#define FORMSET_GUID  {0xA04A27f4,0xDF00,0x4D42,0xB5,0x52,0x39,0x51,0x13,0x02,0x11,0x3D}
```

### #include

This command tells the VFR compiler to use the contents of a file as part of
the source to compile.  **Example:**

```c
#include "C:\Source\DriverSampleStrDefs.h"
```

<!--- @file
  2.11 VFR Form Definition

  Copyright (c) 2007-2017, Intel Corporation. All rights reserved.<BR>
  (C) Copyright 2020 Hewlett Packard Enterprise Development LP<BR>

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

## 2.11 VFR Form Definition

```c
vfrFormDefinition ::=
  "form" "formid" "=" Number ","
  "title" "=" getStringId ";"
  (
      vfrStatementImage
    | vfrStatementLocked
    | vfrStatementRules
    | vfrStatementDefault
    | vfrStatementStat
    | vfrStatementQuestions
    | vfrStatementConditional
    | vfrStatementLabel
    | vfrStatementBanner
    | vfrStatementExtension
    | vfrStatementModal
  )*
"endform" ";"
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `formid` must be unique for each form statement in a given formset.
**********

#### Example

None.

### 2.11.1 VFR Form Map Definition

```c
vfrFormMapDefinition ::=
  "formmap" "formid" "=" Number ","
  (
    "maptitle" "=" getStringId ";"
    "mapguid" "=" guidDefinition ";"
  )*
  (
      vfrStatementImage
    | vfrStatementLocked
    | vfrStatementRules
    | vfrStatementDefault
    | vfrStatementStat
    | vfrStatementQuestions
    | vfrStatementConditional
    | vfrStatementLabel
    | vfrStatementBanner
    | vfrStatementExtension
    | vfrStatementModal
  )*
  "endform" ";"
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `formid` must be unique for each form statement in a given formset.
**********

#### Example

None.

### 2.11.2 VFR Image Statement Definition

```c
vfrStatementImage ::=
  vfrImageTag ";"
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

### 2.11.3 VFR Locked Statement Definition

```c
vfrStatementLocked ::=
  vfrLockedTag ";"
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

### 2.11.4 VFR Rule Statement Definition

```c
vfrStatementRules ::=
  "rule" StringIdentifier ","
    vfrStatementExpression
  "endrule" ";"
```

#### BEHAVIORS AND RESTRICTIONS:

`StringIdentifier` is the name that can be referenced by a question. It should
be unique in the formset.

#### Example

```c
rule MyRule, 1 + 2
endrule;
```

### 2.11.5 VFR Statement Definition

```c
vfrStatementStat ::=
    vfrStatementSubTitle
  | vfrStatementStaticText
  | vfrStatementCrossReference
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

#### 2.11.5.1 VFR SubTitle Definition

```c
vfrStatementSubTitle ::=
  "subtitle"
  "text" "=" getStringId
  { "," "flags" "=" vfrSubtitleFlags }
  (
    { "," vfrStatementStatTagList } ";"
  |
    { "," vfrStatementStatTagList }
    { "," (vfrStatementStat | vfrStatementQuestions)*}
    "endsubtitle" ";"
  )

vfrSubtitleFlags ::=
  subtitleFlagsField ( "|" subtitleFlagsField )* ";"

subtitleFlagsField ::=
    Number
  | "HORIZONTAL"
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `flags` is optional, and the default value is 0.
**********

#### Example

```c
subtitle
  text = STRING_TOKEN(STR_SUBTITLE_TEXT),
  flags = HORIZONTAL;

subtitle
  text = STRING_TOKEN(STR_SUBTITLE_TEXT),

  text
    help  = STRING_TOKEN(STR_TEXT_TEXT),
    text  = STRING_TOKEN(STR_TEXT_TEXT);
    flags = RESET_REQUIRED,
    key   = 0x0001;

endsubtitle;
```

#### 2.11.5.2 VFR Text Definition

```c
vfrStatementStaticText ::=
  "text"
  "help" "=" getStringId ","
  "text" "=" getStringId
  { "," "text" "=" getStringId }
  {
    "," "flags" "=" staticTextFlagsField ( "|" staticTextFlagsField )*
    "," "key" "=" Number
  }
  { "," vfrStatementStatTagList } ";"

staticTextFlagsField ::=
    Number
  | questionheaderFlagsField
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `flags` is optional. The default value is 0.
**********

If `EFI_IFR_FLAGS_CALLBACK` is set in `flags` then it will generate an
`EFI_IFR_ACTION` op-code. Otherwise, it generates the `EFI_IFR_TEXT` op-code.

The value of `key` will be used as a question ID.

#### Example

Generates `EFI_IFR_TEXT`:

```c
text
  help = STRING_TOKEN(STR_TEXT_TEXT),
  text = STRING_TOKEN(STR_TEXT_TEXT);
```

Generates `EFI_IFR_ACTION`:

```c
text
  help  = STRING_TOKEN(STR_TEXT_TEXT),
  text  = STRING_TOKEN(STR_TEXT_TEXT);
  flags = RESET_REQUIRED,
  key   = 0x0001;
```

#### 2.11.5.3 VFR Cross Reference Type Statements Definition

```c
vfrStatementCrossReference ::=
    vfrStatementGoto
  | vfrStatementResetButton
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

##### 2.11.5.3.1 VFR Goto Statement Definition

```c
vfrStatementGoto ::=
  "goto"
  {
    (
      "devicePath"  "=" getStringId ","
      "formsetguid" "=" guidDefinition ","
      "formid"   "=" Number ","
      "question" "=" Number ","
    )
    |
    (
      "formsetguid" "=" guidDefinition ","
      "formid"   "=" Number ","
      "question" "=" Number ","
    )
    |
    (
      "formid"   "=" Number ","
      "question" "="
      (
          StringIdentifier ","
        | Number ","
      )
    )
    |
    (
      Number ","
    )
  }
  vfrQuestionHeader
  { "," "flags" "=" vfrGotoFlags }
  { "," "key" "=" Number }
  { "," vfrStatementQuestionOptionList }
  ";"

vfrGotoFlags ::=
  gotoFlagsField ( "|" gotoFlagsField )*

gotoFlagsField ::=
    Number
  | questionheaderFlagsField
```

#### BEHAVIORS AND RESTRICTIONS

The value of `key` is used as a question ID.

#### Example

Generate `EFI_IFR_REF` without `key`

```c
goto 1,
  prompt = STRING_TOKEN(STR_GOTO_PROMPT),
  help   = STRING_TOKEN(STR_GOTO_HELP);
```

Generate `EFI_IFR_REF` with `key`

```c
goto 1,
  prompt = STRING_TOKEN(STR_GOTO_PROMPT),
  help   = STRING_TOKEN(STR_GOTO_HELP);
  key    = 0x1234;
```

Generate `EFI_IFR_REF2`

```c
goto
  formid   = 1,
  question = QuesttionRef,
  prompt   = STRING_TOKEN(STR_GOTO_PROMPT),
  help     = STRING_TOKEN(STR_GOTO_HELP);
```

Generate `EFI_IFR_REF3`

```c
goto
  formsetguid = FORMSET_GUID,
  formid      = 1,
  question    = QuesttionRef,
  prompt      = STRING_TOKEN(STR_GOTO_PROMPT),
  help        = STRING_TOKEN(STR_GOTO_HELP);
```

Generate `EFI_IFR_REF4`

```c
goto
  devicepath  = STRING_TOKEN(STR_DEVICE_PATH),
  formsetguid = FORMSET_GUID,
  formid      = 1,
  question    = QuesttionRef,
  prompt      = STRING_TOKEN(STR_GOTO_PROMPT),
  help        = STRING_TOKEN(STR_GOTO_HELP);
```

Generate `EFI_IFR_REF5` without `varid`

```c
goto
  prompt = STRING_TOKEN(STR_GOTO_PROMPT),
  help   = STRING_TOKEN(STR_GOTO_HELP);
```

Generate `EFI_IFR_REF5` with `varid`

```c
goto
  varid   = MySTestData.mFieldRef,
  prompt  = STRING_TOKEN(STR_GOTO_PROMPT),
  help    = STRING_TOKEN STR_GOTO_HELP);
  default = FID;QID;GuidValue;STRING_TOKEN(STR_DEVICE_PATH),
  ;
```

Generate `EFI_IFR_REF` with option code

```c
goto 1,
  prompt = STRING_TOKEN(STR_GOTO_PROMPT),
  help   = STRING_TOKEN(STR_GOTO_HELP),
  refresh interval = 3
  ;
```

##### 2.11.5.3.2 VFR ResetButton Statement Definition

```c
vfrStatementResetButton ::=
  "resetbutton"
  "defaultStore" "=" StringIdentifier ","
  vfrStatementHeader ","
  { vfrStatementStatTagList "," }
  "endresetbutton" ";"
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `defaultStore` should point to the default store defined before.
**********

#### Example

```c
resetbutton
  defaultstore = DefaultStoreRef,
  prompt = STRING_TOKEN(STR_RESET_BUTTON_PROMPT),
  help   = STRING_TOKEN(STR_RESET_BUTTON_HELP),
endresetbutton;
```

### 2.11.6 VFR Question Type Statements Definition

```c
vfrStatementQuestions ::=
    vfrStatementBooleanType
 | vfrStatementDate
 | vfrStatementNumericType
 | vfrStatementStringType
 | vfrStatementOrderedList
 | vfrStatementTime
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

#### 2.11.6.1 VFR Question Tag Definition

```
vfrStatementQuestionTag ::=
    vfrStatementStatTag ","
  | vfrStatementInconsistentIf
  | vfrStatementNoSubmitIf
  | vfrStatementDisableIfQuest
  | vfrStatementRefresh
  | vfrStatementVarstoreDevice
  | vfrStatementExtension
  | vfrStatementRefreshEvent
  | vfrStatementWarningIf
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

##### 2.11.6.1.1 VFR Question Tag InconsistentIf Definition

```c
vfrStatementInconsistentIf ::=
  "inconsistentif"
  "prompt" "=" getStringId ","
  vfrStatementExpression
  "endif"
```

#### BEHAVIORS AND RESTRICTIONS

It can only be used in questions.

#### Example

```c
checkbox
  name       = MyCheckBox,
  varid      = MySTestData.mField1,
  questionid = 0xcb,
  prompt     = STRING_TOKEN(STR_CHECK_BOX_PROMPT),
  help       = STRING_TOKEN(STR_CHECK_BOX_HELP),
  flags      = CHECKBOX_DEFAULT | INTERACTIVE,

  inconsistentif
    prompt = STRING_TOKEN(STR_INCONSISTENT_IF),
    ideqval MySTestData.mField1 == 2007
  endif
endcheckbox;
```

##### 2.11.6.1.2 VFR Question Tag NoSubmitIf Definition

```c
vfrStatementNoSubmitIf ::=
  "nosubmitif"
  "prompt" "=" getStringId ","
  vfrStatementExpression
  "endif"
```

#### BEHAVIORS AND RESTRICTIONS

It can only be used in questions.

#### Example

```c
checkbox
  name       = MyCheckBox,
  varid      = MySTestData.mField1,
  questionid = 0xcb,
  prompt     = STRING_TOKEN(STR_CHECK_BOX_PROMPT),
  help       = STRING_TOKEN(STR_CHECK_BOX_HELP),
  flags      = CHECKBOX_DEFAULT | INTERACTIVE,

  nosubmitif prompt = STRING_TOKEN(STR_NOSUBMIT_IF),
    ideqval MySTestData.mField1 == 2007
  endif
endcheckbox;
```

##### 2.11.6.1.3 VFR Question Tag DisableIf Definition

```c
vfrStatementDisableIfQuest ::=
  "disableif" vfrStatementExpression ";"
  vfrStatementQuestionOptionList
  "endif"
```

#### BEHAVIORS AND RESTRICTIONS

None.

#### Example

```c
checkbox
  name       = MyCheckBox,
  varid      = MySTestData.mField1,
  questionid = 0xcb,
  prompt     = STRING_TOKEN(STR_CHECK_BOX_PROMPT),
  help       = STRING_TOKEN(STR_CHECK_BOX_HELP),
  flags      = CHECKBOX_DEFAULT | INTERACTIVE,

  disableif
    ideqvallist MySTestData.mField1 == 1 3 5 7;
    refresh interval = 1
  endif
endcheckbox;
```

##### 2.11.6.1.4 VFR Question Tag Refresh Definition

```c
vfrStatementRefresh ::=
  "refresh" "interval" "=" Number
```

#### BEHAVIORS AND RESTRICTIONS

It can only be used in questions.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 0xff,
        refresh interval = 3
endnumeric;
```

##### 2.11.6.1.5 VFR Question Tag VarstoreDevice Definition

```c
vfrStatementVarstoreDevice ::=
  "varstoredevice" "=" getStringId ","
```

#### BEHAVIORS AND RESTRICTIONS

It can only be used in questions.

#### Example

```c
checkbox
  name       = MyCheckBox,
  varid      = MySTestData.mField1,
  questionid = 0xcb,
  prompt     = STRING_TOKEN(STR_CHECK_BOX_PROMPT),
  help       = STRING_TOKEN(STR_CHECK_BOX_HELP),
  flags      = CHECKBOX_DEFAULT | INTERACTIVE,

  varstoredevice = STRING_TOKEN(STR_VARSTOREDEVICE),
endcheckbox;
```

##### 2.11.6.1.6 VFR Question Tag RefreshEvent Definition

```c
vfrStatementRefreshEvent ::=
  "refreshguid" "=" guidDefinition ","
```

#### BEHAVIORS AND RESTRICTIONS

It can only be used in questions.

#### Example

```c
numeric
  varid       = MySTestData.mField2,
  prompt      = STRING_TOKEN(STR_NUMERIC_PROMPT),
  help        = STRING_TOKEN(STR_NUMERIC_HELP),
  flags       = DISPLAY_UINT_HEX,
  minimum     = 0,
  maximum     = 300,
  step        = 0,
  refreshguid = EFI_IFR_REFRESH_ID_OP_GUID,
  default     = 175,
endnumeric;
```

##### 2.11.6.1.7 VFR Question Tag WarningIf Definition vfrStatement

```c
WarningIf ::=
  "warningif" "prompt" "=" getStringId "," {"timeout" "=" Number ","}
  vfrStatementExpression
  "endif"
```

#### BEHAVIORS AND RESTRICTIONS

It can only be used in questions.

#### Example

```c
checkbox
  name       = MyCheckBox,
  varid      = MySTestData.mField1,
  questionid = 0xcb,
  prompt     = STRING_TOKEN(STR_CHECK_BOX_PROMPT),
  help       = STRING_TOKEN(STR_CHECK_BOX_HELP),
  flags      = CHECKBOX_DEFAULT | INTERACTIVE,

  warningif prompt = STRING_TOKEN(STR_INCONSISTENT_IF), timeout = 5,
    ideqval MySTestData.mField1 == 2007
  endif
endcheckbox;
```

#### 2.11.6.2 VFR Question Tag List Definition

```c
vfrStatementQuestionTagList ::=
  ( vfrStatementQuestionTag )*
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

#### 2.11.6.3 VFR Question Option Tag Definition

```c
vfrStatementQuestionOptionTag ::=
    vfrStatementSuppressIfQuest
  | vfrStatementValue
  | vfrStatementDefault
  | vfrStatementOptions
  | vfrStatementRead
  | vfrStatementWrite
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

##### 2.11.6.3.1 VFR Question SuppressIf Statement Definition

```c
vfrStatementSuppressIfQuest ::=
  "suppressif" vfrStatementExpression ";"
    vfrStatementQuestionOptionList
  "endif"
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

##### 2.11.6.3.2 VFR Default Statement Definition

```c
vfrStatementDefault ::=
  "default"
  (
    (
        vfrStatementValue ","
      | "=" vfrConstantValueField ","
    )
    { "defaultstore" "=" StringIdentifier "," }
  )
```

#### BEHAVIORS AND RESTRICTIONS

It can only be used in a question definition.

**********
**Note:** `defaultstore` is optional and it points to the default store defined
previously. If `defaultstore` is not defined, the
`EFIHII_DEFAULT_CLASS_STANDARD` is assigned.
**********

#### Example

```c
default = 1,
default value = 1 + 2,
default = {1,2,3},
```

##### 2.11.6.3.3 VFR Value Statement Definition

```c
vfrStatementValue ::=
  "value" "=" vfrStatementExpression ";"
```

#### BEHAVIORS AND RESTRICTIONS

None.

#### Example

```c
Value = 0;
```

##### 2.11.6.3.4 VFR Option Type Statements Definition

```c
vfrStatementOptions ::=
  vfrStatementOneOfOption
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

##### 2.11.6.3.5 VFR OneOf Option Statement Definition

```c
vfrStatementOneOfOption ::=
  "option"
  "text"  "=" getStringId ","
  "value" "=" vfrConstantValueField ","
  "flags" "=" vfrOneOfOptionFlags
  ( "," vfrImageTag )*
  ";"

vfrOneOfOptionFlags ::=
  oneofoptionFlagsField ( "|" oneofoptionFlagsField )*

oneofoptionFlagsField ::=
    Number
  | "OPTION_DEFAULT"
  | "OPTION_DEFAULT_MFG"
  | "INTERACTIVE"
  | "RESET_REQUIRED"
  | "DEFAULT"
  | "REST_STYLE"
```

#### BEHAVIORS AND RESTRICTIONS

An `option` statement is special; it is used to embellish or describe questions.

These statements can be used to give the possible value and set the default
value for questions. In other words, they are not questions, but they influence
the questions they embellish. Therefore, the options' `flags` are treated as
question `flags` and can accept all values of question `flags`.

Options with the DEFAULT flags can be used to set the default value for
questions.

#### Example

```c
option text = STRING_TOKEN(STR_ONE_OF_TEXT), value = 0x2, flags = DEFAULT | RESET_REQUIRED;
```

##### 2.11.6.3.6 VFR Read Statement Definition

```c
vfrStatementRead ::=
  "read" vfrStatementExpression ";"
```

#### BEHAVIORS AND RESTRICTIONS

None.

#### Example

None

##### 2.11.6.3.7 VFR Write Statement Definition

```c
vfrStatementWrite ::=
  "write" vfrStatementExpression ";"
```

#### BEHAVIORS AND RESTRICTIONS

None.

#### Example

None

#### 2.11.6.4 VFR Question Tag List Definition

```c
vfrStatementQuestionOptionList ::=
  (
      vfrStatementQuestionTag
    | vfrStatementQuestionOptionTag
  )*
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

#### 2.11.6.5 VFR Boolean Type Statement Definition

```c
vfrStatementBooleanType ::=
    vfrStatementCheckBox
  | vfrStatementAction
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

##### 2.11.6.5.1 VFR CheckBox Statement Definition

```c
vfrStatementCheckBox ::=
  "checkbox"
  vfrQuestionHeader ","
  { "flags" "=" vfrCheckBoxFlags "," }
  { "key" "=" Number "," }
  vfrStatementQuestionOptionList
  "endcheckbox" ";"

vfrCheckBoxFlags ::=
  checkboxFlagsField ( "|" checkboxFlagsField )*

checkboxFlagsField ::=
    Number
  | "CHECKBOX_DEFAULT"
  | "CHECKBOX_DEFAULT_MFG"
  | questionheaderFlagsField
```

#### BEHAVIORS AND RESTRICTIONS

The value of `key` is used as question ID.

**********
**Note:** `flags` is optional, and the default value is 0.
**********

#### Example

```c
checkbox
  name    = MyCheckBox,
  varid   = MySTestData.mField1,
  prompt  = STRING_TOKEN(STR_CHECK_BOX_PROMPT),
  help    = STRING_TOKEN(STR_CHECK_BOX_HELP),
  flags   = CHECKBOX_DEFAULT | INTERACTIVE,
  default = TRUE,
endcheckbox;
```

##### 2.11.6.5.2 VFR Action Statement Definition

```c
vfrStatementAction ::=
  "action"
  vfrQuestionHeader ","
  { "flags" "=" vfrActionFlags "," }
  "config" "=" getStringId ","
  vfrStatementQuestionTagList
  "endaction" ";"

vfrActionFlags ::=
  actionFlagsField ( "|" actionFlagsField )*

actionFlagsField ::=
    Number
  | questionheaderFlagsField
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `flags` is optional, and the default value is 0.
**********

#### Example

```c
action
  prompt = STRING_TOKEN(STR_ACTION_PROMPT),
  help   = STRING_TOKEN(STR_ACTION_HELP),
  flags  = INTERACTIVE,
  config = STRING_TOKEN(STR_ACTION_CONFIG),
endaction;
```

#### 2.11.6.6 VFR Numeric Type Statements Definition

```c
vfrStatementNumericType ::=
    vfrStatementNumeric
  | vfrStatementOneOf
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

##### 2.11.6.6.1 VFR Numeric Statement Definition

```c
vfrStatementNumeric ::=
  "numeric"
  vfrQuestionHeader ,
  { "flags" "=" vfrNumericFlags "," }
  { "key" "=" Number "," }
  vfrSetMinMaxStep
  vfrStatementQuestionOptionList
  "endnumeric" ";"

vfrSetMinMaxStep ::=
  "minimum" "=" Number ","
  "maximum" "=" Number ","
  { "step"  "=" Number "," }

vfrNumericFlags ::=
  numericFlagsField ( "|" numericFlagsField )*

numericFlagsField ::=
    Number
  | "NUMERIC_SIZE_1"
  | "NUMERIC_SIZE_2"
  | "NUMERIC_SIZE_4"
  | "NUMERIC_SIZE_8"
  | "DISPLAY_INT_DEC"
  | "DISPLAY_UINT_DEC"
  | "DISPLAY_UINT_HEX"
  | questionheaderFlagsField
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `flags` is optional, and the default value partly depends on the size
of `varid` defined in `vfrQuestionHeader`.
**********

The default display format is `DISPLAY_UINT_DEC`.

#### Example

```c
numeric
  varid   = STestData.mField2,
  prompt  = STRING_TOKEN(STR_NUMERIC_PROMPT),
  help    = STRING_TOKEN(STR_NUMERIC_HELP),
  flags   = DISPLAY_UINT_HEX,
  minimum = 0,
  maximum = 300,
  step    = 0,
  default = 175,
endnumeric;
```

##### 2.11.6.6.2 VFR OneOf Statement Definition

```c
vfrStatementOneOf ::=
  "oneof"
  vfrQuestionHeader,
  { "flags" "=" vfrOneofFlagsField "," }
  { vfrSetMinMaxStep }
  vfrStatementQuestionOptionList
  "endoneof" ";"

vfrOneofFlagsField ::=
  numericFlagsField ( "|" numericFlagsField )*
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `flags` is optional, and the default value partly depends on the size
of `varid` defined in `vfrQuestionHeader` syntax.
**********

The flag is defined in the VFR Numeric Statement Definition.

#### Example

```c
oneof
  varid  = STestData.mField3[0],
  prompt = STRING_TOKEN(STR_ONE_OF_PROMPT),
  help   = STRING_TOKEN(STR_ONE_OF_HELP),
  flags  = DISPLAY_UINT_DEC,

  option text = STRING_TOKEN(STR_ONE_OF_TEXT1), value = 0x0, flags = 0;
  option text = STRING_TOKEN(STR_ONE_OF_TEXT2), value = 0x1, flags = 0;
  option text = STRING_TOKEN(STR_ONE_OF_TEXT3), value = 0x2, flags = DEFAULT;
endoneof;
```

#### 2.11.6.7 VFR String Type Statements Definition

```c
vfrStatementStringType ::=
    vfrStatementString
  | vfrStatementPassword
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

##### 2.11.6.7.1 VFR String Statement Definition

```c
vfrStatementString ::=
  "string"
  vfrQuestionHeader ","
  { "flags" "=" vfrStringFlagsField "," }
  { "key" "=" Number "," }
  "minsize" "=" Number ","
  "maxsize" "=" Number ","
  vfrStatementQuestionOptionList
  "endstring" ";"

vfrStringFlagsField ::=
  stringFlagsField ( "|" stringFlagsField )*

stringFlagsField ::=
    Number
  | "MULTI_LINE"
  | questionheaderFlagsField
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `flags` is optional, and the default value is 0.
**********

#### Example

```c
string
  varid   = STestData.mField1,
  prompt  = STRING_TOKEN(STR_MY_STRING_PROMPT),
  help    = STRING_TOKEN(STR_MY_STRING_HELP),
  flags   = MULTI_LINE,
  minsize = 6,
  maxsize = 0x14,
endstring;
```

##### 2.11.6.7.2 VFR Password Statement Definition

```c
vfrStatementPassword ::=
  "password"
  vfrQuestionHeader ,
  { "flags" "=" vfrPasswordFlagsField "," }
  { "key" "=" Number "," }
  "minsize" "=" Number ","
  "maxsize" "=" Number ","
  vfrStatementQuestionOptionList
  "endpassword" ";"

vfrPasswordFlagsField ::=
  passwordFlagsField ( "|" passwordFlagsField )*

passwordFlagsField ::=
    Number
  | questionheaderFlagsField
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `flags` is optional, and the default value is 0.
**********

The value of `key` is used as a question ID.

#### Example

```c
password
  varid    = STestData.mPrivate.mField2,
  prompt  = STRING_TOKEN(STR_PASSWORD_PROMPT),
  help    = STRING_TOKEN(STR_PASSWORD_HELP),
  minsize = 6,
  maxsize = 20,
endpassword;
```

#### 2.11.6.8 VFR OrderedList Statement Definition

```c
vfrStatementOrderedList ::=
  "orderedlist"
  vfrQuestionHeader ","
  { "maxcontainers" "=" Number "," }
  { "flags" "=" vfrOrderedListFlags }
  vfrStatementQuestionOptionList
  "endlist" ";"

vfrOrderedListFlags ::=
   orderedlistFlagsField ( "|" orderedlistFlagsField )*

orderedlistFlagsField ::=
    Number
 | "UNIQUE"
 | "NOEMPTY"
 | questionheaderFlagsField
```

#### BEHAVIORS AND RESTRICTIONS

**********
**Note:** `maxcontainers` is optional, and the default value depends on the
variable size defined by `varid` in `vfrQuestionHeader`.
**********
**Note:** `flags` is optional, and the default value is 0.
**********

#### Example

```c
orderedlist
  varid  = STestPriData.mField3,
  prompt = STRING_TOKEN(STR_BOOT_OPTIONS),
  help   = STRING_TOKEN(STR_BOOT_OPTIONS),

  option text = STRING_TOKEN(STR_BOOT_OPTION2), value = 2, flags = RESET_REQUIRED;
  option text = STRING_TOKEN(STR_BOOT_OPTION1), value = 1, flags = RESET_REQUIRED;
  option text = STRING_TOKEN(STR_BOOT_OPTION3), value = 3, flags = RESET_REQUIRED;
  option text = STRING_TOKEN(STR_BOOT_OPTION4), value = 4, flags = RESET_REQUIRED;
  option text = STRING_TOKEN(STR_EMPTY_STRING), value = {1, 2, 3, 4}, flags = DEFAULT;
endlist;
```

#### 2.11.6.9 VFR Date Statement Definition

```c
vfrStatementDate ::=
  "date"
  (
    (
      vfrQuestionHeader ,
      { "flags" "=" vfrDateFlags "," }
      vfrStatementQuestionOptionList
    )
    |
    (
      "year" "varid" "=" StringIdentifier "." StringIdentifier ","
      "prompt" "=" getStringId ","
      "help" "=" getStringId ","
      minMaxDateStepDefault

      "month" "varid" "=" StringIdentifier "." StringIdentifier ","
      "prompt" "=" getStringId ","
      "help" "=" getStringId ","
      minMaxDateStepDefault

      "day" "varid" "=" StringIdentifier "." StringIdentifier ","
      "prompt" "=" getStringId ","
      "help" "=" getStringId ","
      minMaxDateStepDefault

      ( vfrStatementInconsistentIf )*
    )
  )
  "enddate" ";"

minMaxDateStepDefault ::=
  "minimum" "=" Number ","
  "maximum" "=" Number ","
  { "step" "=" Number "," }
  { "default" "=" Number "," }

vfrDateFlags ::=
  dateFlagsField ( "|" dateFlagsField )*

dateFlagsField ::=
    Number
  | "YEAR_SUPPRESS"
  | "MONTH_SUPPRESS"
  | "DAY_SUPPRESS"
  | "STORAGE_NORMAL"
  | "STORAGE_TIME"
  | "STORAGE_WAKEUP"
```

#### BEHAVIORS AND RESTRICTIONS

There are old and new syntax of the VFR Date statement, but they are
incompatible. The old VFR Date only uses EFI Date/Time Storage, instead of
normal question value storage. The new VFR Date can use either the normal
question value storage or EFI Date/Time Storage.

For the new syntax of VFR, `flags` is optional, and the default value is 0
Examples follow.

**New syntax of VFR Date:**

```c
date
  varid   = STestData.mDate,
  prompt  = STRING_TOKEN(STR_DATE_PROMPT),
  help    = STRING_TOKEN(STR_DATE_PROMPT),
  flags   = STORAGE_NORMAL,
  default = 2007/08/26,
enddate;
```

**Old syntax of VFR Date:**

```c
date year varid       = Date.Year,
          prompt      = STRING_TOKEN(STR_DATE_PROMPT),
          help        = STRING_TOKEN(STR_DATE_HELP),
          minimum     = 2003,
          maximum     = 2100,
          step        = 1,
          default     = 2003,

          month varid = Date.Month
          prompt      = STRING_TOKEN(STR_DATE_PROMPT),
          help        = STRING_TOKEN(STR_DATE_HELP),
          minimum     = 1,
          maximum     = 12,
          step        = 1,
          default     = 1,

          day varid   = Date.Day,
          prompt      = STRING_TOKEN(STR_DATE_PROMPT),
          help        = STRING_TOKEN(STR_DATE_HELP),
          minimum     = 1,
          maximum     = 31,
          step        = 0x1,
          default     = 1,
enddate;
```

#### 2.11.6.10 VFR Time Statement Definition

```c
vfrStatementTime :
  "time"
  (
    (
      vfrQuestionHeader ","
      { "flags" "=" vfrTimeFlags "," }
      vfrStatementQuestionOptionList
    )
    |
    (
      "hour" "varid" "=" StringIdentifier "." StringIdentifier ","
      "prompt" "=" getStringId ","
      "help" "=" getStringId ","
      minMaxTimeStepDefault

      "minute" "varid" "=" StringIdentifier "." StringIdentifier ","
      "prompt" "=" getStringId ","
      "help" "=" getStringId ","
      minMaxTimeStepDefault

      "second" "varid" "=" StringIdentifier "." StringIdentifier ","
      "prompt" "=" getStringId ","
      "help" "=" getStringId ","
      minMaxTimeStepDefault

      ( vfrStatementInconsistentIf )*
    )
  )
  "endtime" ";"

minMaxTimeStepDefault ::=
  "minimum"   "=" Number ","
  "maximum"   "=" Number ","
  { "step"    "=" Number "," }
  { "default" "=" Number "," }

vfrTimeFlags ::=
  timeFlagsField ( "|" timeFlagsField )*

timeFlagsField ::=
    Number
  | "HOUR_SUPPRESS"
  | "MINUTE_SUPPRESS"
  | "SECOND_SUPPRESS"
  | "STORAGE_NORMAL"
  | "STORAGE_TIME"
  | "STORAGE_WAKEUP"
```

#### BEHAVIORS AND RESTRICTIONS:

There are old and new syntax of VFR Time statements. They are incompatible.

* **New:** VFR Time can use either the normal question value storage or EFI
  Date/Time Storage.

* **Old:** VFR Date can use only EFI Date/Time Storage, instead of normal
  question value storage.

In the new syntax of VFR, `flags` is optional, and the default value is 0

#### Example

**New Time Syntax:**

```c
time
      name    = MyTime,
      varid   = STestData.mTime,
      prompt  = STRING_TOKEN(STR_TIME_PROMPT),
      help    = STRING_TOKEN(STR_TIME_PROMPT),
      flags   = STORAGE_NORMAL,
      default = 15:33:33,
endtime;
```

**Old Time Syntax:**

```c
time hour varid = Time.Hour,
     prompt     = STRING_TOKEN(STR_TIME_PROMPT),
     help       = STRING_TOKEN(STR_TIME_HELP),
     minimum    = 0,
     maximum    = 23,
     step       = 1,
     default    = 0,

     minute varid = Time.Minute,
     prompt       = STRING_TOKEN(STR_TIME_PROMPT),
     help         = STRING_TOKEN(STR_TIME_HELP),
     minimum      = 0,
     maximum      = 59,
     step         = 1,
     default      = 0,

     second varid = Time.Second,
     prompt       = STRING_TOKEN(STR_TIME_PROMPT),
     help         = STRING_TOKEN(STR_TIME_HELP),
     minimum      = 0,
     maximum      = 59,
     step         = 1,
     default      = 0,
endtime;
```

### 2.11.7 VFR Conditional Type Statements Definition

```c
vfrStatementConditional ::=
    vfrStatementDisableIfStat
  | vfrStatementSuppressIfStat
  | vfrStatementGrayOutIfStat
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTION or an Example for this section.
**********

#### 2.11.7.1 VFR Statement List Definition

```c
vfrStatementStatList ::=
    vfrStatementStat
  | vfrStatementQuestions
  | vfrStatementConditional
  | vfrStatementLabel
  | vfrStatementExtension
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

#### 2.11.7.2 VFR Statement DisalbeIf Definition

```c
vfrStatementDisableIfStat ::=
  "disableif" vfrStatementExpression ";"
  ( vfrStatementStatList )*
  "endif" ";"
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

#### 2.11.7.3 VFR Statement SuppressIf Definition

```c
vfrStatementSuppressIfStat ::=
  "suppressif" vfrStatementExpression ";"
  ( vfrStatementStatList )*
  "endif" ";"
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

#### 2.11.7.4 VFR Statement GrayOutIf Definition

```c
vfrStatementGrayOutIfStat ::=
  "grayoutif" vfrStatementExpression ";"
  ( vfrStatementStatList )*
  "endif" ";"
```

**********
**Note:** There are no BEHAVIORS AND RESTRICTIONS or an Example for this
section.
**********

### 2.11.8 VFR GUID Statement Definition

#### 2.11.8.1 VFR Label Statement Definition

```c
vfrStatementLabel ::=
  "label" Number ";"
```

#### BEHAVIORS AND RESTRICTIONS

It is an extended EFI_IFR_GUID op-code.

It should be paired in the VFR program.

**********
**Note:** `label` is used to insert op-codes at runtime.
**********

#### Example

```c
label LABEL_START;
label LABEL_END;
```

#### 2.11.8.2 VFR Banner Statement Definition

```c
vfrStatementBanner ::=
  "banner" { "," }
  "title" "=" getStringId ","
  (
    (
      "line" Number ","
      "align" ( "left" | "center" | "right" ) ";"
    )
    |
    ( "timeout" "=" Number ";" )
  )
```

#### BEHAVIORS AND RESTRICTIONS

It is an extended EFI_IFR_GUID op-code.

#### Example

```c
banner
  title = STRING_TOKEN(STR_BANNER_TITLE),
  line 1,
  align center;
```

#### 2.11.8.3 VFR GUID Extension Definition

```c
vfrStatementExtension ::=
  "guidop"
  "guid" "=" guidDefinition
  {
    "," "datatype" "="
    (
        "UINT64" { "[" Number "]" }
      | "UINT32" { "[" Number "]" }
      | "UINT16" { "[" Number "]" }
      | "UINT8" { "[" Number "]" }
      | "BOOLEAN" { "[" Number "]" }
      | "EFI_STRING_ID" { "[" Number "]" }
      | "EFI_HII_DATE" { "[" Number "]" }
      | "EFI_HII_TIME" { "[" Number "]" }
      | "EFI_HII_REF" { "[" Number "]" }
      | StringIdentifier { "[" Number "]" }
    )
    vfrExtensionData
  }
  {
    "," ( vfrStatementExtension )*
    "endguidop"
  }
  ";"

vfrExtensionData ::=
  (
    "," "data" { "[" Number "]" }
    (
      "." StringIdentifier { "[" Number "]" }
    )*
    "=" Number
  )*
```

#### BEHAVIORS AND RESTRICTIONS

The generic guidop statement is used to specify user extension opcodes. By
default, unassigned byte will be zero. The array number in the "data" part
cannot be equal to or larger than the one in the "datatype" part. That is, the
array index starts from zero.

#### Example

```c
guidop
  guid = GUID,
  datatype = UINT32,
  data = 0x12345678;
```

### 2.11.9 VFR Modal Statement Definition

```c
vfrStatementModal ::=
  modal";"
```

#### BEHAVIORS AND RESTRICTIONS

It is only used in a form.

#### Example

```c
modal;
```

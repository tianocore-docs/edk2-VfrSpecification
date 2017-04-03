<!--- @file
  2.12 VFR Expression Statement Definition

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

## 2.12 VFR Expression Statement Definition

The VFR expression is defined in C-style language.

The syntax of VFR expression is defined as a tree. The positions in the tree
are determined according to the priority of the operator (for example: + - \*
/). At the root of it are the terms of `OR`, followed by the terms of `AND`,
because the priority of operator `OR` is lower than the operator `AND`(s). The
leaves of the tree are sub- expressions of built-in-functions, unary operators,
ternary operators, and constants.

### 2.12.1 OR

```c
vfrStatementExpression ::=
  andTerm ( "OR" andTerm )*
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_OR` op-codes.

### 2.12.2 AND

```c
andTerm ::=
  bitwiseorTerm ( "AND" bitwiseorTerm )*
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_AND` op-codes.

### 2.12.3 bitwiseor

```c
bitwiseorTerm ::=
  bitwiseandTerm ( "|" bitwiseandTerm )*
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_BITWISE_OR` op-codes.

### 2.12.4 bitwiseand

```c
bitwiseandTerm ::=
  equalTerm ( "&" equalTerm )*
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_BITWISE_AND` op-codes.

### 2.12.5 equal

```c
equalTerm ::=
  compareTerm
  (
      "==" compareTerm
    | "!=" compareTerm
  )*
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_EQUAL` or `EFI_IFR_NOT_EQUAL` op-codes.

### 2.12.6 compare

```c
compareTerm ::=
  shiftTerm
  (
      "<" shiftTerm
    | "<=" shiftTerm
    | ">" shiftTerm
    | ">=" shiftTerm
  )*
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_LESS_THAN EFI_IFR_LESS_EQUAL, EFI_IFR_IFR_GREATER_EQUAL,` or
`EFI_IFR_GREATER_THAN` op-codes.

### 2.12.7 shift

```c
shiftTerm ::=
  addMinusTerm
  (
      "<<" addMinusTerm
    | ">>" addMinusTerm
  )*
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_SHIFT_LEFT` or `EFI_IFR_SHIFT_RIGHT` op-codes.

### 2.12.8 add/minus

```c
addMinusTerm ::=
  multdivmodTerm
  (
      "+" multdivmodTerm
    | "-" multdivmodTerm
  )*
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_ADD` or `EFI_IFR_SUBTRACT` op-codes.

### 2.12.9 multiply/divide/modulo

```c
multdivmodTerm ::=
  castTerm
  (
      "*" castTerm
    | "/" castTerm
    | "%" castTerm
  )*
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_MULTIPLY EFI_IFR_MODULO` or `EFI_IFR_DIVIDE` op-codes.

### 2.12.10 cast terms

```c
castTerm ::=
  (
    "("
    (
        "BOOLEAN"
      | "UINT64"
      | "UINT32"
      | "UINT16"
      | "UINT8"
    )
    ")"
  )*
  atomTerm
```

#### BEHAVIORS AND RESTRICTIONS

The VFR supports the C-style type conversion. The values can be converted into
one of the following types: `BOOLEAN`, `UINT64`, `UINT32`, `UINT16`,  `UINT8`.

### 2.12.11 atom terms

```c
atomTerm ::=
    vfrExpressionCatenate
  | vfrExpressionMatch
  | vfrExpressionParen
  | vfrExpressionBuildInFunction
  | vfrExpressionConstant
  | vfrExpressionUnaryOp
  | vfrExpressionTernaryOp
  | vfrExpressionMap
  | ( "NOT" atomTerm )
  | vfrExpressionMatch2
```

#### 2.12.11.1 catenate

```c
vfrExpressionCatenate ::=
  "catenate"
  "(" vfrStatementExpression "," vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_CATENATE` op-codes.

#### Example

```c
string  varid   = MyData.String,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minsize = 6,
        maxsize = 40,
        inconsistentif prompt = STRING_TOKEN(STR_CHECK),
           pushthis != catenate(stringref(STRING_TOKEN(STR_STRING_RIGHT)),
                                stringref(STRING_TOKEN(STR_STRING_LEFT))),
        endif
endstring;
```

#### 2.12.11.2 match

```c
vfrExpressionMatch ::=
  "match"
  "(" vfrStatementExpression "," vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_MATCH` op-codes.

#### Example

```c
grayoutif  match(stringref(STRING_TOKEN(STR_PATTERN)),
                 stringref(STRING_TOKEN(STR_STRING)))
      numeric varid   = MyData.Data,
              prompt  = STRING_TOKEN(STR_PROMPT),
              help    = STRING_TOKEN(STR_HELP),
              minimum = 0,
              maximum = 243,
      endnumeric;
endif;
```

#### 2.12.11.3 parenthesis

```c
vfrExpressionParen ::=
  "(" vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Changes the order of the calculation.

#### 2.12.11.4 build-in functions

```c
vfrExpressionBuildInFunction ::=
    dupExp
  | ideqvalExp
  | ideqidExp
  | ideqvallistExp
  | questionref1Exp
  | rulerefExp
  | stringref1Exp
  | pushthisExp
  | securityExp
  | getExp
```

##### 2.12.11.4.1 dup

```c
dupExp ::=
  "dup"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_DUP` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 0xf0,
          default value = 2 + dup,
endnumeric;
```

##### 2.12.11.4.2 ideqval

```c
ideqvalExp ::=
  "ideqval"
  vfrQuestionDataFieldName "==" Number
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_EQ_ID_VAL` op-codes.

#### Example

```c
grayoutif ideqval MyData.Data1 == 99;
  numeric varid   = MyData.Data,
          prompt  = STRING_TOKEN(STR_PROMPT),
          help    = STRING_TOKEN(STR_HELP),
          minimum = 0,
          maximum = 0xf0,
  endnumeric;
endif;
```

##### 2.12.11.4.3 ideqid

```c
ideqidExp ::=
  "ideqid"
  vfrQuestionDataFieldName "==" vfrQuestionDataFieldName
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_EQ_ID_ID` op-codes.

#### Example

```c
grayoutif ideqid MyData.Data2 == MyData.Data3;
  numeric varid   = MyData.Data,
          prompt  = STRING_TOKEN(STR_PROMPT),
          help    = STRING_TOKEN(STR_HELP),
          minimum = 0,
          maximum = 0xf0,
  endnumeric;
endif;
```

##### 2.12.11.4.4 ideqvallist

```c
ideqvallistExp ::=
  "ideqvallist"
  vfrQuestionDataFieldName "==" ( Number )+
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_EQ_ID_LIST` op-codes.

```c
grayoutif ideqvallist MyData.Data1 == 1 3;
  numeric name    = MyNumeric,
          varid   = MyData.Data,
          prompt  = STRING_TOKEN(STR_PROMPT),
          help    = STRING_TOKEN(STR_HELP),
          minimum = 0,
          maximum = 0xf0,
  endnumeric;
endif;
```

##### 2.12.11.4.5 questionref

```c
questionref1Exp ::=
  "questionref"
    "(" StringIdentifier | Number ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_QUESTION_REF1` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 0xf0,
        default value = questionref(MyNumeric),
endnumeric;
```

##### 2.12.11.4.6 ruleref

```c
rulerefExp ::=
  "ruleref" "(" StringIdentifier ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_RULE_REF` op-codes.

#### Example

```c
grayoutif ruleref(MyRule) == 1;
  string varid   = MyData.String,
         prompt  = STRING_TOKEN(STR_PROMPT),
         help    = STRING_TOKEN(STR_HELP),
         minsize = 6,
         maxsize = 40,
  endstring;
endif;
```

##### 2.12.11.4.7 stringref

```c
stringref1Exp ::=
  "stringref" "(" getStringId ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_STRING_REF1` op-codes.

#### Example

```c
grayoutif stringref(STRING_TOKEN(STR_STRING)) == 1;
  string varid   = MyData.String,
         prompt  = STRING_TOKEN(STR_PROMPT),
         help    = STRING_TOKEN(STR_HELP),
         minsize = 6,
         maxsize = 40,
  endstring;
endif;
```

##### 2.12.11.4.8 pushthis

```c
pushthisExp ::=
  "pushthis"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_THIS` op-codes.

#### Example

```c
string  varid   = MyData.String,
        prompt  = STRING_TOKEN(STR_PROMPT), help = STRING_TOKEN(STR_HELP),
        minsize = 6,
        maxsize = 40,
        inconsistentif prompt = STRING_TOKEN(STR_CHECK),
          pushthis != stringref(STRING_TOKEN(STR_STRING))
        endif
endstring;
```

##### 2.12.11.4.9 security

```
securityExp ::=
  "security" "(" guidDefinition ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_SECURITY` op-codes.

#### Example

```c
grayoutif NOT security(EFI_GUID);
    text
      help = STRING_TOKEN(STR_HELP),
      text = STRING_TOKEN(STR_TEXT);
endif;
```

##### 2.12.11.4.10 get

```
getExp ::=
  "get" "(" vfrStorageVarId { "|" "flags" "=" vfrNumericFlags } ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_GET` op-codes.

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        read get(MyData.Data1);
endnumeric;
```

#### 2.12.11.5 constant

```c
vfrExpressionConstant ::=
    "TRUE"
  | "FALSE" | "ONE"
  | "ONES"
  | "ZERO"
  | "UNDEFINED"
  | "VERSION"
  | Number
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_TRUE EFI_IFR_FALSE`, `EFI_IFR_ONE EFI_IFR_ONES`,
`EFI_IFR_ZERO`, `EFI_IFR_UNDEFINED`, or `EFI_IFR_VERSION` op-codes.

####2.12.11.6 unary operators

```c
vfrExpressionUnaryOp ::=
    lengthExp
  | bitwisenotExp
  | question23refExp
  | stringref2Exp
  | toboolExp
  | tostringExp
  | unintExp
  | toupperExp
  | tolwerExp
  | setExp
```

##### 2.12.11.6.1 length

```c
lengthExp ::=
  "length" "(" vfrStatementExpression ")"
```
#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_LENGTH` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value = length(stringref(STRING_TOKEN(STR_STRING))),
endnumeric;
```

##### 2.12.11.6.2 bitwisenot

```c
bitwisenotExp ::=
  "~" "(" vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_BITWISENOT` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value = ~(length(stringref(STRING_TOKEN(STR_STRING)))),
endnumeric;
```

##### 2.12.11.6.3 questionrefval

```c
question23refExp ::=
  "questionrefval"
  "("
    {
      DevicePath "=" "STRING_TOKEN" "\(" S:Number "\)" ","
    }
    {
      Uuid "=" guidDefinition[Guid] ","
    }
    vfrStatementExpression
  ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_QUESTION_REF2` or `EFI_IFR_QUESTION_REF3` op-codes.

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value = questionrefval(QuestionID),
endnumeric;
```

##### 2.12.11.6.4 stringrefval

```c
stringref2Exp ::=
  "stringrefval" "(" vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_STRING_REF2` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value = length(stringrefval(STR_STRING)),
endnumeric;
```

##### 2.12.11.6.5 boolval

```c
toboolExp ::=
  "boolval" "(" vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_TO_BOOLEAN` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255, default value = boolval(12),
endnumeric;
```

##### 2.12.11.6.6 stringval

```
tostringExp ::=
  "stringval" { "format" "=" Number "," }
  "(" vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_TO_STRING` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value = length(stringval(format = 8, 12)),
endnumeric;
```

##### 2.12.11.6.7 unintval

```c
unintExp ::=
  "unintval" "(" vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_TO_UINT` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value = unintval(12 * 3),
endnumeric;
```

##### 2.12.11.6.8 toupper

```c
toupperExp ::=
  "toupper" "(" vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_TO_UPPER` op-codes.

#### Example

```c
grayoutif length(toupper(stringref(STRING_TOKEN(STR_STRING))))==1;
  string varid   = MyData.String,
         prompt  = STRING_TOKEN(STR_PROMPT),
         help    = STRING_TOKEN(STR_HELP),
         minsize = 6,
         maxsize = 40,
  endstring;
endif;
```

##### 2.12.11.6.9 tolower

```c
tolwerExp ::=
  "tolower" "(" vfrStatementExpression ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_TO_LOWER` op-codes.

```c
grayoutif length(tolower(stringref(STRING_TOKEN(STR_STRING))))==2;
  string varid   = MyData.String,
         prompt  = STRING_TOKEN(STR_PROMPT),
         help    = STRING_TOKEN(STR_HELP),
         minsize = 6,
         maxsize = 40,
  endstring;
endif;
```

##### 2.12.11.6.10 set

```c
setExp ::=
  "set"
  "("
  vfrStorageVarId { "|" "flags" "=" vfrNumericFlags } ","
  vfrStatementExpression
  ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_SET` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        write set(MyData.Data1,10);
endnumeric;
```

##### 2.12.11.7 ternary operators

```c
vfrExpressionTernaryOp ::=
    conditionalExp
  | findExp
  | midExp
  | tokenExp
  | spanExp
```

##### 2.12.11.7.1 cond

```c
conditionalExp ::=
  "cond"
  "("
  vfrStatementExpression1
  "?"
  vfrStatementExpression2
  ":"
  vfrStatementExpression3
  ")"
```

#### BEHAVIORS AND RESTRICTIONS

`If (Expr1) then x = Expr3 else Expr2`

Generates `EFI_IFR_CONDITIONAL` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value = cond(2 == 1 ? 5 : 10),
endnumeric;
```

##### 2.12.11.7.2 find

```c
findExp ::=
  "find"
  "("
  findFormat ( "|" findFormat )*
  ","
  vfrStatementExpression
  ","
  vfrStatementExpression
  ","
  vfrStatementExpression
  ")"

findFormat ::=
    "SENSITIVE"
  | "INSENSITIVE"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_FIND` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value =
          find(INSENSITIVE, (stringref(STRING_TOKEN(STR_STRING1))),
                            (stringref(STRING_TOKEN(STR_STRING2))), 1),
endnumeric;
```

##### 2.12.11.7.3 mid

```c
midExp ::=
  "mid"
  "("
  vfrStatementExpression
  ","
  vfrStatementExpression
  ","
  vfrStatementExpression
  ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_MID` op-codes.

#### Example

```c
grayoutif length(mid(stringref(STRING_TOKEN(STR_STRING)),6,8))==1;
  string varid   = MyData.String,
         prompt  = STRING_TOKEN(STR_PROMPT),
         help    = STRING_TOKEN(STR_HELP),
         minsize = 6,
         maxsize = 40,
  endstring;
endif;
```

##### 2.12.11.7.4 tok

```c
tokenExp ::=
  "token"
  "("
  vfrStatementExpression
  ","
  vfrStatementExpression
  ","
  vfrStatementExpression
  ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_TOKEN` op-codes.

#### Example

```c
grayoutif length(token(stringref(STRING_TOKEN(STR_STRING1)),
                 stringref(STRING_TOKEN(STR_STRING2)), 0)) == 2;
  string varid   = MyData.String,
         prompt  = STRING_TOKEN(STR_PROMPT),
         help    = STRING_TOKEN(STR_HELP),
         minsize = 6,
         maxsize = 40,
  endstring;
endif;
```

##### 2.12.11.7.5 span

```c
spanExp ::=
  "span"
  "("
  "flags" "=" spanFlags ( "|" spanFlags )*
  ","
  vfrStatementExpression
  ","
  vfrStatementExpression
  ","
  vfrStatementExpression
  ")"

spanFlags ::=
    Number
  | "LAST_NON_MATCH"
  | "FIRST_NON_MATCH"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_SPAN` op-codes.

#### Example

```c
numeric varid = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value = span(flags = LAST_NON_MATCH,
                             stringref(STRING_TOKEN(STR_STRING1)),
                             stringref(STRING_TOKEN(STR_STRING2)),0),
endnumeric;
```

#### 2.12.11.8 map

```c
vfrExpressionMap ::=
  "map"
  "("
  vfrStatementExpression
  ":"
  (
    vfrStatementExpression
    ","
    vfrStatementExpression
    ";"
  )*
  ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_MAP` op-codes.

#### Example

```c
numeric varid   = MyData.Data,
        prompt  = STRING_TOKEN(STR_PROMPT),
        help    = STRING_TOKEN(STR_HELP),
        minimum = 0,
        maximum = 255,
        default value = map(pushthis:0,10;1,2;3,5;6,8;),
endnumeric;
```

#### 2.12.11.9 match2

```c
vfrExpressionMatch ::=
  "match2"
  "("
  vfrStatementExpressionPattern
  ","
  vfrStatementExpressionString
  ","
  guidDefinition[Guid]
  ")"
```

#### BEHAVIORS AND RESTRICTIONS

Generates `EFI_IFR_MATCH2` op-codes.

#### Example

```c
grayoutif match2(stringref(STRING_TOKEN(STR_PATTERN)),
                 stringref(STRING_TOKEN(STR_STRING)),
                 PERL_GUID);
    numeric varid   = MyData.Data,
            prompt  = STRING_TOKEN(STR_PROMPT),
            help    = STRING_TOKEN(STR_HELP),
            minimum = 0,
            maximum = 243,
    endnumeric;
endif;
```

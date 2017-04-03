<!--- @file
  1 Introduction

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

# 1 Introduction

## 1.1 Overview

Internal Forms Representation (IFR) is a binary encoding. This encoding is
designed to be comparatively easy to parse and to manipulate programmatically.

To simplify the creation of IFR, a high-level Visual Forms Representation (VFR)
language is defined below. Using this language syntax, a compiler can be
designed to take an ordinary text file containing VFR as an input, and output
IFR for use in a user's program. There are various methods to define the VFR
language. This document sets out a method and describes it using BNF-style
syntax.

## 1.2 Assumed Knowledge

A full understanding of the _UEFI Specification_ is assumed throughout this
document.

This document is a reference manual for developers adopting the VFR language to
create products compliant with current UEFI Specification. It offers enough
material to create infrastructure files for the user interface, and to create a
driver to export setup- related information to the Human Interface
Infrastructure programming interface.

## 1.3 Related Information

* _Unified Extensible Firmware Interface Specification_, Unified EFI, Inc.,
  http://www.uefi.org

## 1.4 Terms

The following terms are used throughout this document to describe varying
aspects of input localization:

**BNF**

BNF is an acronym for "Backus Naur Form". John Backus and Peter Naur
introduced for the first time a formal notation to describe the syntax of a
given language.

**HII**

Human Interface Infrastructure. This generally refers to the database that
contains string, font, and IFR information along with other pieces that use one
of the database components.

**IFR**

Internal Forms Representation. This is the binary encoding that is used for
the representation of user interface pages.

**VFR**

Visual Forms Representation. This is the source code format that is used by
developers to create a user interface with varying pieces of data or questions.
This is later compiled into a binary encoding

Microsoft Research Detours Package, Version 1.5 (Build 46)


DISCLAIMER AND LICENSE:
=======================
The entire Detours package is covered by copyright law.
Copyright 1995-2001, Microsoft Corporation.  Portions of the Detours package
may be covered by patents owned by Microsoft Corporation.  All rights reserved.

Usage of the Detours package is covered under the End User License Agreement in
the file License.doc.  Your usage of Detours implies your acceptance of the End
User License Agreement.  A copy of the license in ASCII can be found in the
file LICENSE.TXT.  A complete list of redistributable files is in REDIST.TXT.


INTRODUCTION:
=============
The file detours.doc contains a paper describing the Detours package.


COMPATIBILITY:
==============
All Detours functions are compatible with all x86 version of Windows NT,
Windows 2000, and Windows XP.  However, under Windows 95, Windows 98, and
Windows ME, the DetourFunction* APIS do not work unless the program is
running under a debugger (the process was created with the DEBUG_PROCESS flag
on the call to the CreateProcess* APIs).  Since most programs are not typically
run under a debugger, the DetourFunction* APIs do not work for most programs
on Win9x platforms.

While Windows NT, Windows 2000 and Windows XP always map DLLs into processes
with copy-on-write mapping (which Detours needs in order to patch the binary
image), Windows 95, Windows 98, and Windows ME only map DLLs with copy-on-write
if the process was started with the DEBUG_PROCESS flag on the call to
CreateProcess.

The DLL injection functions are not compatible with either Windows 95,
Windows 98, or Windows ME as the Win9x platforms do not supports the
CreateRemoteThread API.

The binary rewriting function for adding payloads and modifying import tables
are fully compatible with Windows 95, Windows 98, Windows ME, Windows NT,
Windows 2000, and Windows XP.


BUILD INSTRUCTIONS:
===================
To build the libraries and the sample applications, type "nmake".


VERIFYING THE INSTALL AND BUILD:
================================
After building the libraries and sample applications, you can verify that
the Detours packet works on your Windows OS by running the Test.bat program
in the samples\slept directory.  The output of Test.bat should be similar
to that contained in the file samples\slept\NORMAL.TXT.


MANIFEST:
=========
The Detours package current consists of the Detours library (with or without
source code) and a number of sample programs.  Descriptions of the sample
programs can be found in samples\README.TXT


NOTES:
======
When writing detour functions, it is imperative that the binary-calling
convention of the detour and trampoline functions match exactly the
binary-calling convention of the target function.

In a few cases, when the sizeof() a return value is smaller than sizeof(int),
C or C++ compilers will generate non-compatible binary-calling conventions by
not widening the return value to an int as is customary for small return values.
The result is a syntactically-identical, but not binary-compatible, detour
function.  In most cases, the problem can be fixed be having the detour function
return a value widened to a sizeof(int) type.  Developers are urged to exercise
caution, and should insure that correct code is generated by their C or C++
compiler for detour functions with small return values.

When attaching a DLL to a binary with Detours DLL import APIs, the DLL must
export one procedure with export ordinal 1.  The exported procedure is not
called by the application, but it used as the import target.


BUG REPORTS:
============
Please send detailed bug reports to detours@microsoft.com.  Submitted bug
reports may be used to fix bugs in future versions of the Detours package.
The detours@microsoft.com email address is not a product support alias.

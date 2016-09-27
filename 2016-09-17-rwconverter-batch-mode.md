---
layout: post
title:  "rwconverter utility in batch mode - issues"
date:   2016-09-17
categories: ["Oracle Reports"]
tags: Reports
author: "Ali Rqiq"
---

>rwconverter utility (11g - release 2) converts one or more report definitions or PL/SQL libraries from one storage format to another.
In this article we take as an example a conversion from rdf to rep files.

#### Modify rwconverter.bat
>I had some issues running this tool in batch mode. The first conversion in the  **for loop** when completed did not exit and the conversion could not go to the second one.
To circumvent this issue I had to modify the file rwconverter.bat located at ORACLE_INSTANCE\config\reports\bin.

The resulting modification is as follows:

```
@REM
@REM Copyright (c) 2001, 2008, Oracle and/or its affiliates.
@REM All rights reserved.
@REM

@echo off
@echo Starting Reports 11g Converter...
setlocal
:: call $$Instance.directory$$\config\reports\bin\reports.bat
call C:\Oracle\asinst_1\config\reports\bin\reports.bat
@echo on
@REM Please do not use start keyword for running rwconverter in batch mode
@REM Uncomment the below line for running in the batch mode and comment out the other one
@REM $$Instance.oracle_home$$\bin\rwconverter.exe %*
:: start $$Instance.oracle_home$$\bin\rwconverter.exe %*
call start C:\Oracle\Oracle_FRHome1\BIN\rwconverter.exe %*
exit
@echo off
endlocal
@echo on
```

#### Create the batch mode script
Create a script such as convert.bat and add these lines:
```
@echo off
set ORACLE_HOME=C:\Oracle\Oracle_FRHome1
set PATH=C:\Oracle\Oracle_FRHome1\BIN;%PATH%
set FORMS_PATH=C:\Oracle\Oracle_FRHome1\forms;C:\formsLab\admin;C:\formsLab\commonlib;C:\formsLab\templates

FOR %%F in (*.rdf) DO START /W /Min C:\Oracle\asinst_1\config\reports\bin\rwconverter.bat USERID=dbuser/password@db_host/SID STYPE=rdffile SOURCE=%%F DTYPE=repfile DEST=%%F OVERWRITE=YES BATCH=YES

@echo Done !
:END
```

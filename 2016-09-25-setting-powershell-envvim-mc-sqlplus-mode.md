---
layout: post
title:  "Setting Microsoft Powershell environment for vi, git, sqlplus, java and mc"
date:   2016-09-25
categories: ["Powershell"]
tags: PowerShell
author: "Ali Rqiq"
---

> This is how I set my Powershell environment in [Windows 7](https://www.microsoft.com/en-ca/software-download/windows7){:target="_blank"} so I can execute, out of the box, [sqlplus](http://ss64.com/ora/syntax-sqlplus.html){:target="_blank"}, [mc](https://sourceforge.net/projects/mcwin32/){:target="_blank"}, [vi](http://www.vim.org/download.php){:target="_blank"}, [git](https://git-scm.com/download/win){:target="_blank"} and compile [java](https://docs.oracle.com/javase/8/docs/){:target="_blank"} codes. I suppose that all these sodtware are installed ;-) Right ?

You need to edit the PowerShell Profile file which is named ``Microsoft.PowerShell_profile.ps1`` and located at ``%USERPROFILE%\Documents\WindowsPowerShell``.

Here is how my profile file looks like:
``` powershell
## DB access from powershell - call sqlplus from the command line
Set-Alias show Get-ChildItem
$env:Path = "C:\oraclexe\app\oracle\product\11.2.0\server\bin"
$env:ORACLE_HOME = "C:\oraclexe\app\oracle\product\11.2.0\server"
$env:ORACLE_SID = "XE"

## Making Powershell aware of Java environment
$env:Path = "C:\oraclexe\app\oracle\product\11.2.0\server\bin;C:\javas\1.8\jdk\bin"
$env:JAVA_HOME = "C:\javas\1.8\jdk"

$GITPATH = "C:\Users\arqiq\software\Git\bin\git.exe"
Set-Alias git $GITPATH

## vi integration in powershell
$VIMPATH    = "C:\Apps\Vim\vim80\vim.exe"

Set-Alias vi   $VIMPATH
Set-Alias vim  $VIMPATH

#
set shell=powershell
set shellcmdflag=-command

## Midnigh Commander (mc) (file manager) integration
$MCPATH    = "C:\Program Files (x86)\Midnight Commander\mc.exe"
Set-Alias mc   $MCPATH

```
Hope this helps !

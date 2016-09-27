---
layout: post
title:  "How to determine ADF Version in Oracle Fusion Middleware"
date:   2016-08-01
categories: ADF
tags: ADF
author: "Ali Rqiq"
---
> There are way to find out the version of an installed adf in Oracle Fusion Middelware. Here are some.  

##### Possibility one:
~~~ bash
$ export ORACLE_HOME=/u01/oracle/fusion//oracle_common
$ cd $ORACLE_HOME/modules
$ unzip -p oracle.adf.share_11.1.1/adf-share-support.jar | tr '[\000-\011\013-\037\177-\377]' '.'  | grep JDEVADF
~~~  
The output will look like:
``Oracle-Label: JDEVADF_11.1.1.6.0_GENERIC_111205.1733.6192.1``

##### Possiblity two:
~~~ bash  
$ export ORACLE_HOME=/u01/oracle/fusion//oracle_common
$ cd $ORACLE_HOME/modules/oracle.adf.model_11.1.1
$ java -cp adfm.jar oracle.jbo.common.PrintVersion
~~~  
The output will look like:
``BC4J Version is: 11.1.1.61.92``
The four last digits is the Build number. Please refer to Document: 401694.1 Oracle JDeveloper Releases for correlation of the Build number with the JDeveloper release.
In this example 61.92 refers to 11.1.1.6.0  

##### Possibility three:
~~~ bash  
$ export ORACLE_HOME=/u01/oracle/fusion//oracle_common
$ cd $ORACLE_HOME/OPatch
$ ./opatch lsinventory -details | grep JDEVADF
~~~  
The output will look like:
``JDEVADF 11.1.1.6.0``
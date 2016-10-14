---
layout: post
title:  "WebLogic Thin T3 Client"
date:   2016-10-02
categories: ["Weblogic"]
tags: Weblogic
author: "Ali Rqiq"
---

>Weblogic full clients are Java [RMI](https://docs.oracle.com/cd/E24329_01/web.1211/e24389/rmi_basics.htm#WLRMI118){:target="_blank"} clients that use Oracle's proprietary [T3 protocol](https://docs.oracle.com/cd/E24329_01/web.1211/e24389/rmi_t3.htm#WLRMI143){:target="_blank"} to communicate with WebLogic Server.

For Weblogic Server 10.0 up to 12.1.2, client applications need to use the wlfullclient.jar file instead of the weblogic.jar.
wlfullclient.jar is not available by default, so you need to create it using the WebLogic JarBuilder Tool (wljarbuilder.jar). wlfullclient.jar and cryptoj.jar should reside in the same location as the wlfullcient.jar references cryptoj.jar in its manifest Class-Path.

Once wlfullclient.jar is build, add it to the client application's classpath.

Starting from the WebLogic 12.1.3 release, you need to use the available wlthint3client.jar which is a light-weight, high performing alternative to the wlfullclient.jar and wlclient.jar (IIOP) remote client jars.

Here is how to build the wlfullclient.jar:
``` bash
$ cd $WL_HOME/server/lib
$ ls wlfullclient.jar
$ ls wlthint3client.jar
ls: cannot access wlfullclient.jar: No such file or directory
$ ls cryptoj.jar
cryptoj.jar
$ java -jar wljarbuilder.jar
Creating new jar file: wlfullclient.jar
```

![Video illustration]({{ site.url }}/assets/videos/weblogic_fullclient.gif)
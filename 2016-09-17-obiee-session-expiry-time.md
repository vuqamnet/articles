---
layout: post
title:  "How to change the session expiry time in OBIEE"
date:   2016-09-17
categories: OBIEE
tags: ["OBIEE"]
author: "Ali Rqiq"
---

The safest and easiest way to change the session expiry time is by going to the Oracle Enterprise Manager Fusion Middleware Control.

Here are the steps  to follow:
Click or Activate a tab to go from one step to another
1.	Business Intelligence
2.	coreapplication
3.	Lock and Edit Configuration
4.	Capacity Management
5.	 Performance
6.	Go to the section User Session Expiry and adjust your desired time in minutes
7.	Apply
8.	Activate Changes
9.	Close the Confirmation window
10.	Restart to apply recent changes
11.	Restart all
12.	Confirm by Yes
13.	Close the Confirmation window

You can download some screenshots that depict all those steps. [My helpful screenshots]({{ site.url }}/assets/files/obi_session_imeout.zip)

You can otherwise change the session expiry time by editing the file instanceconfig.xml located in this directory ``$ORACLE_INSTANCE/config/OracleBIPresentationServicesComponent/coreapplication_obips1 ``

Inside the Security node, change the value of ClientSessionExpireMinutes. For example a setting of 60 minutes looks like this:
``` xml
<Security>
<ClientSessionExpireMinutes>60</ClientSessionExpireMinutes>
</Security>
```
Save the file and restart the OracleBI Presentation Services Component using opmnctl.
``` bash
$ $ORACLE_INSTANCE/bin/opmnctl restartproc ias-component=coreapplication_obips1
```
Hope this helps !
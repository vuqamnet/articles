---
layout: post
title:  "How to reset the admin password in WebLogic 11g and 12c"
date:   2016-09-07
categories: Weblogic
tags: Weblogic
author: "Ali Rqiq"
---
>It is simple to reset the admin password in WebLogic 11g and 12c under Linux. 
Suppose that ``weblogic`` is the Weblogic administrator and ``weblogic2016`` is its new password.

The following commands are just examples applied to the environment where Weblogic is running. Adapt accordinly to your situation.

~~~ bash
$ export MW_HOME=/apps/oracle/fusionBI
$ export DOMAIN_HOME=$MW_HOME/user_projects/domains/bi_domain
$ $DOMAIN_HOME/bin/stopWebLogic.sh
$ mv $DOMAIN_HOME/servers/AdminServer/data $DOMAIN_HOME/servers/AdminServer/data-old
$ . $DOMAIN_HOME/bin/setDomainEnv.sh
$ cd $DOMAIN_HOME/security
$ mv DefaultAuthenticatorInit.ldift DefaultAuthenticatorInit.ldift.old
$ java weblogic.security.utils.AdminAccount weblogic weblogic2016 .
~~~

Update the “$DOMAIN_HOME/servers/AdminServer/security/boot.properties”
username=weblogic
password=weblogic2015

Now start Weblogic Admin Server.
~~~ bash
$ $DOMAIN_HOME/bin/startWebLogic.sh
~~~
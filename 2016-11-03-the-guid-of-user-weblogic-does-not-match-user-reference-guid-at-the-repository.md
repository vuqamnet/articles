---
layout: post
title:  "The GUID of user 'weblogic' does not match user reference GUID at the repository"
date:   2016-11-03
categories: ["Weblogic", "OBIEE"]
tags: OBIEE
author: "Ali Rqiq"
---

> The user weblogic can no longer log in through Oracle BI Publisher (11.1.1.7.141014) after hosting a new repository.

``` bash
$ cd $ORACLE_INSTANCE/diagnostics/logs/OracleBIServerComponent/coreapplication_obis1
$ tail -f nqserver.log
```
```
rmissions></runAsUser></result></ns3:impersonateUserWithLanguageAndPropertiesResponse></env:Body></env:Envelope>
[2016-11-03T13:53:28.000-04:00] [OracleBIServerComponent] [ERROR:1] [] [] [ecid: 2d9f25ece1efb40e:64d74d:1582b239b95:-8000-000000000000124a,0:1:1:6] [tid: fdb2c700]  [nQSError: 13041] The GUID of user 'weblogic' does not match user reference GUID at the repository. Please ask the administrator to delete the old user reference at the repository and login again.
[2016-11-03T13:53:28.000-04:00] [OracleBIServerComponent] [ERROR:1] [] [] [ecid: 2d9f25ece1efb40e:64d74d:1582b239b95:-8000-000000000000124a,0:1:1:6] [tid: fdb2c700]  [nQSError: 43126] Authentication failed: invalid user/password.
[2016-11-03T13:56:31.000-04:00] [OracleBIServerComponent] [NOTIFICATION:1] [] [] [ecid: 005G6^j8SVCApIG6yzzW6G0005lF000000,0:81:6] [tid: fd92a700] User 'BISystemUser' spent 15.000000 milliseconds for http response when getAuthenticatedUserWithLanguageAndProperties
```
The error output indicated that I needed to remove the user weblogic from the repository which indeed solved the issue.

Using Oracle BI Administration Tool, I removed the user weblogic from the RPD.
Saved it and hosted it using the Enterprise Manager Console.

![image 0]({{ site.url }}/assets/images/posts/weblogicmatch/weblogic_does_not_match_00.png)

![image 1]({{ site.url }}/assets/images/posts/weblogicmatch/weblogic_does_not_match_01.png)

![image 2]({{ site.url }}/assets/images/posts/weblogicmatch/weblogic_does_not_match_02.png)

![image 3]({{ site.url }}/assets/images/posts/weblogicmatch/weblogic_does_not_match_03.png)

![image 4]({{ site.url }}/assets/images/posts/weblogicmatch/weblogic_does_not_match_04.png)

![image 5]({{ site.url }}/assets/images/posts/weblogicmatch/weblogic_does_not_match_05.png)

Hope this helps !
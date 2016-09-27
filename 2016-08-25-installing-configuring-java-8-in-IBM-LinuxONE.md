---
layout: post
title:  "Installing and configuring Java 8 in IBM LinuxONE (s390x)"
date:   2016-08-25
categories: Java
tags: Java IBM  LinuxONE Linux
author: "Ali Rqiq"
---

> I just requested a new IBM LinuxONE Community Cloud account hosted at Marist College and started to setup my new instance based on Red Hat Enterprise Linux Server 7.2 (Maipo). I always liked to set up my Linux environment by first updating, adding and removing packages. It seemed that Java was not installed by default. So here are these easy and straightforward steps to install Java 8.

As root,
~~~ bash
$ yum install java java-devel javapackages-tools
~~~

As of my writing, I have installed the following versions:

``Package 1:java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x already installed and latest version``

``Package 1:java-1.8.0-ibm-devel-1.8.0.2.10-1jpp.1.el7.s390x already installed and latest version``

``Package javapackages-tools-3.4.1-11.el7.noarch already installed and latest version``


#### Install Java with Alternatives
~~~ bash
$ alternatives --install /usr/bin/java java /usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/jre/bin/java 2
$ alternatives --config java

There is 1 program that provides 'java'.
  Selection    Command
-----------------------------------------------
*+ 1           /usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/jre/bin/java
 
Enter to keep the current selection[+], or type selection number: 1
~~~

#### Set up javac and jar paths using alternatives
~~~ bash
$ ln -s /usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/bin/javac /etc/alternatives/
$ alternatives --install /usr/bin/javac javac /usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/bin/javac 2
$ alternatives --set javac /usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/bin/javac
$ ln -s /usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/bin/jar /etc/alternatives/
$ alternatives --install /usr/bin/jar jar /usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/bin/jar 2
$ alternatives --set jar /usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/bin/jar  
~~~  
  
#### Make Java available to all users
Edit /etc/environment as root and add the following lines:
~~~ bash
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x
export JRE_HOME=/usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/jre
export PATH=/usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/bin:/usr/lib/jvm/java-1.8.0-ibm-1.8.0.2.10-1jpp.1.el7.s390x/jre/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/$USER/.local/bin:/$USER/bin
~~~

---
layout: post
title:  "Install Ruby on Rails with rbenv on CentOS 7"
date:   2016-09-17
categories: Ruby
tags: ["Ruby", "Rails"]
author: "Ali Rqiq"
---

> [Rails](http://rubyonrails.org/){:target="_blank"}, is a server-side web application framework written in [Ruby](https://www.ruby-lang.org/en/){:target="_blank"} under the MIT License which complies with the MVC pattern to organize application programming.

#### Install [rbenv](https://github.com/rbenv/rbenv){:target="_blank"}
As root,
``` bash
# yum install git-core zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel
```
As a non root user do the following:
``` bash
$ cd
$ git clone git://github.com/sstephenson/rbenv.git .rbenv
$ vi ~/.bash_profile
```
Add these lines:    
``export PATH="$HOME/.rbenv/bin:$PATH"``  
``eval "$(rbenv init -)"``  
``export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"``  
``` bash
$ exec $SHELL (this command replaces the current shell process image)
```
``` bash
$ git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
$ echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bash_profile
$ exec $SHELL
$ source ~/.bash_profile
```
#### Install ruby 2.3.1 :
As a non root user,
``` bash
$ rbenv install -v 2.3.1
$ rbenv global 2.3.1
$ ruby -v
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-linux]
```
I bet you don't want Ruby gems to generate local documentation for every single gem you install. To do so:
``` bash
$ echo "gem: --no-document" > ~/.gemrc
```
Install [Bundler](https://bundler.io/){:target="_blank"} which intelligently allows you to track and install with all necessary dependencies the exact gems needed for your ruby projects.
``` bash
$ gem install bundler
Fetching: bundler-1.13.0.gem (100%)
Successfully installed bundler-1.13.0
1 gem installed
```

### Install rails 5.0.0.1
``` bash
$ gem install rails -v 5.0.0.1
$ rbenv rehash   (to hook into the environment's PATH)
$ rails -v
Rails 5.0.0.1
```

#### Optional

Install Javascript Runtime:
As root user,
``` bash
# yum install nodejs
```

Make sure that MariaDB is installed and also make sure that the package maraidb-devel is installed.

As root:
``` bash
# yum install mariadb-devel
```

Then as a non root user,
``` bash
$ gem install mysql2
Successfully installed mysql2-0.4.4
1 gem installed

$ rbenv rehash  
```

#### The moment of truth
``` bash
$ rails new testapp  # this coammand creates a new application called testapp
$ cd testapp
$ rake db:create
$ rails server --binding=617.305.928.677 (just an example, this is not a valid IP anyway ;-) )
=> Booting Puma
=> Rails 5.0.0.1 application starting in development on http://617.305.928.677:3000
=> Run `rails server -h` for more startup options
Puma starting in single mode...
* Version 3.6.0 (ruby 2.3.1-p112), codename: Sleepy Sunday Serenity
* Min threads: 5, max threads: 5
* Environment: development
* Listening on tcp://617.305.928.677:3000
Use Ctrl-C to stop
```

Make sure that the port 3000 is allowed by your system firewall.
As root,
``` bash
# firewall-cmd --permanent --add-port=3000/tcp
# firewall-cmd --reload
```

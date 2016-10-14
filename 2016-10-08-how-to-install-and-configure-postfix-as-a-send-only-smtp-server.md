---
layout: post
title:  "How to install and configure Postfix as a Gmail Relay on Oracle Linux 7.2"
date:   2016-10-08
categories: ["Postfix"]
tags: Postfix
author: "Ali Rqiq"
---
> **Purpose:** I manage some [cloud servers](https://m.do.co/c/fa13be36fda1){:target="_blank"} on which I have installed applications that need to send email notifications. In this case, I have no need to run a full SMTP server for only this minimal purpose. I, then, decided to run a local-send-only SMTP server using [Postfix](http://www.postfix.org/){:target="_blank"} as a [Gmail Relay](https://support.google.com/a/answer/2956491?hl=en){:target="_blank"}.

Please note that you most likely need to create some DNS records. In my case, as I use [Digital Ocean](https://m.do.co/c/fa13be36fda1){:target="_blank"}, the set up is straight forward

At [Digital Ocean](https://m.do.co/c/fa13be36fda1){:target="_blank"}, here is how the zonefile looks like:
```
$ORIGIN cloud-db.space.
$TTL 1800
cloud-db.space. IN SOA ns1.digitalocean.com. hostmaster.cloud-db.space. 1471457063 10800 3600 604800 1800
cloud-db.space. 1800 IN NS ns1.digitalocean.com.
cloud-db.space. 1800 IN NS ns2.digitalocean.com.
cloud-db.space. 1800 IN NS ns3.digitalocean.com.
cloud-db.space. 1800 IN A 192.241.159.17
mail.cloud-db.space. 1800 IN CNAME cloud-db.space.
cloud-db.space. 1800 IN TXT "v=spf1 a include:_spf.google.com ~all"
```
#### Step 1
Create a new specific gmail passowrd, by visiting this [page](https://security.google.com/settings/security/apppasswords?pli=1){:target="_blank"}

![Video illustration]({{ site.url }}/assets/videos/gmail_password.gif)

#### Step 2
Ensure that the following software packages are installed [postfix](http://www.postfix.org/){:target="_blank"}, [mailx](https://engineering.purdue.edu/ECN/Support/KB/Docs/MailXTutorial){:target="_blank"} and [cyrus-sasl-plain](http://www.sendmail.org/~ca/email/cyrus/sysadmin.html)
As root user:
``` bash
# yum install postfix mailx cyrus-sasl-plain
# systemctl enable postfix
# vi /etc/postfix/sasl_passwd
```
Add this line: 
`` smtp.gmail.com    user@gmail.com:rsdmfvnijgrtxtdg``

Replace ``user@gmail.com`` by your real gmail account and ``rsdmfvnijgrtxtdg`` by the password you obtained previously.

The following command will create a file named ``sasl_passwd.db`` and stored in ``/etc/postfix``
As root user:
``` bash
# postmap hash:/etc/postfix/sasl_passwd
```
Add the following lines to the bottom of ``/etc/postfix/main.cf``
```
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_security_level = secure
smtp_tls_mandatory_protocols = TLSv1
smtp_tls_mandatory_ciphers = high
smtp_tls_secure_cert_match = nexthop
smtp_tls_CAfile = /etc/pki/tls/certs/ca-bundle.crt
relayhost = smtp.gmail.com:587
```

Here is a copy of my postfix main.cf:
```
queue_directory = /var/spool/postfix
command_directory = /usr/sbin
daemon_directory = /usr/libexec/postfix
data_directory = /var/lib/postfix
mail_owner = postfix
myhostname = mail.cloud-db.space
mydomain = cloud-db.space
myorigin = $mydomain
inet_interfaces = loopback-only
inet_protocols = ipv4
mydestination = $myhostname, localhost
unknown_local_recipient_reject_code = 550
mynetworks_style = host
relayhost = [smtp.gmail.com]:587
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
debug_peer_level = 2
debugger_command =
         PATH=/bin:/usr/bin:/usr/local/bin:/usr/X11R6/bin
         ddd $daemon_directory/$process_name $process_id & sleep 5

sendmail_path = /usr/sbin/sendmail.postfix
newaliases_path = /usr/bin/newaliases.postfix
mailq_path = /usr/bin/mailq.postfix
setgid_group = postdrop
html_directory = no
manpage_directory = /usr/share/man
sample_directory = /usr/share/doc/postfix-2.10.1/samples
readme_directory = /usr/share/doc/postfix-2.10.1/README_FILES
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_security_level = secure
smtp_tls_mandatory_protocols = TLSv1
smtp_tls_mandatory_ciphers = high
smtp_tls_secure_cert_match = nexthop
smtp_tls_CAfile = /etc/pki/tls/certs/ca-bundle.crt
```
Remove the file sasl_passwd as it contains the password in clear text. The hashed file sasl_passwd.db contained the hashed password that will be used to authenticate your gmail account.
``` bash
# rm /etc/postfix/sasl_passwd
```

Now, we need to set up the forwarding System Mail:
As root user:
``` bash
# vi /etc/aliases
```
Make sure that the following are accordingly  set:
```
mailer-daemon:  postmaster
postmaster:     root
root:           user@gmail.com
```
Please, replace ``user@gmail.com`` by your valid gmail account.
``` bash
# newaliases
# postfix reload
# postfix check
# systemctl restart postfix
```
#### Step 3
You can test your settings by sending an email to someone or to yoursel. For example:
``` bash
# echo "Sent by opdev" | mail -s "Hello Me ..." user@gmail.com
```
This is an example of an output when sending an email:
```
# tail -f /var/log/maillog
Aug 17 19:51:28 oracle-db postfix/pickup[7688]: 87B3C3F5DD: uid=0 from=<root>
Aug 17 19:51:28 oracle-db postfix/cleanup[8066]: 87B3C3F5DD: message-id=<20160815243128.87B3C3F5DD@mail.cloud-db.space>
Aug 17 19:51:28 oracle-db postfix/qmgr[7689]: 87B3C3F5DD: from=<root@cloud-db.space>, size=468, nrcpt=1 (queue active)
Aug 17 19:51:29 oracle-db postfix/smtp[8068]: 87B3C3F5DD: to=<user@gmail.com>, relay=smtp.gmail.com[209.85.144.108]:587, delay=1.3, delays=0.01/0/0.26/1, dsn=2.0.0, status=sent (250 2.0.0 OK 1471463489 m10sm18589615qta.31 - gsmtp)
Aug 17 19:51:29 oracle-db postfix/qmgr[7689]: 87B3C3F5DD: removed
Aug 17 19:52:03 oracle-db postfix/pickup[7688]: 9FDB43F5DD: uid=0 from=<root>
Aug 17 19:52:03 oracle-db postfix/cleanup[8066]: 9FDB43F5DD: message-id=<20160815243128.87B3C3F5DD@mail.cloud-db.space>
Aug 17 19:52:03 oracle-db postfix/qmgr[7689]: 9FDB43F5DD: from=<root@cloud-db.space>, size=476, nrcpt=1 (queue active)
Aug 17 19:52:04 oracle-db postfix/smtp[8068]: 9FDB43F5DD: to=<user@gmail.com>, relay=smtp.gmail.com[209.85.144.108]:587, delay=0.94, delays=0.01/0/0.28/0.65, dsn=2.0.0, status=sent (250 2.0.0 OK 1471463524 c25sm18704339qta.6 - gsmtp)
Aug 17 19:52:04 oracle-db postfix/qmgr[7689]: 9FDB43F5DD: removed
```
Hope this helps !
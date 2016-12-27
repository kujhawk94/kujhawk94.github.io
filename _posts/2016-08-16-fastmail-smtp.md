---
layout: post
title:  Configure postfix to use Fastmail smtp
date: 2016-08-16
last_modified:  2016-12-26
categories: postfix debian fastmail
---

[Fastmail][5] provides outgoing [smtp services][3] to its users via an [app-specific password][4].  Postfix configuration can be set to use Fastmail's servers for smtp with correct authentication headers (dkim, dmarc, spf).  Before starting the postfix configuration, follow Fastmail's directions for creating an [app-specific password][4].

Install `postfix` and associated packages for securing the connection.  Select *Internet Site* during installation and set an appropriate FQDN.

```
$ sudo apt-get install postfix mailutils libsasl2-2 ca-certificates libsasl2-modules
```

Add the following lines to `/etc/postfix/main.cf`

```
relayhost = [smtp.fastmail.com]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CAfile = /etc/postfix/cacert.pem
smtp_use_tls = yes
```

The password is stored in the file `/etc/postfix/sasl_passwd` with the following line.  Replace the username with the email address used to connect to the Fastmail web page.  The password is the app-specific password generated above.

```
[smtp.fastmail.com]:587 username:password
```

Fix the permissions

```
$ sudo chmod 400 /etc/postfix/sasl_passwd
```

And tell postfix about the password entry

```
$ sudo postmap /etc/postfix/sasl_passwd
```

Make a copy of the certificate for postfix if it was installed with the ca-certificates.  If the certificate is not in `/etc/ssl/certs`, download the certificate directly from [Thawte's Root Certificates page][7] ([Thawte Primary Root CA][6]). _Thanks to [Jim Kalafut][8] for noting the Thawte certificate may not be installed._

```
$ sudo cp /etc/ssl/certs/Thawte_Premium_Server_CA.pem /etc/postfix/cacert.pem
```

And reload 

```
$ sudo /etc/init.d/postfix reload
```

Finally, a test email

```
$ echo "Test mail from postfix" | mail -s "Test Postfix" you@example.com
```

> Information above based on a [similar article][1] located on [easyengine.io][2].


[1]: https://easyengine.io/tutorials/linux/ubuntu-postfix-gmail-smtp/
[2]: https://easyengine.io/tutorials/
[3]: https://www.fastmail.com/help/technical/servernamesandports.html#email
[4]: https://www.fastmail.com/help/clients/apppassword.html
[5]: http://www.fastmail.com
[6]: https://www.thawte.com/roots/thawte_Primary_Root_CA.pem 
[7]: https://www.thawte.com/roots/
[8]: https://kalafut.net


[^1]: 
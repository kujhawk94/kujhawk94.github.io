---
layout: post
title: ReadyNAS stuck Frontview
date: 2016-02-10
---
Periodically apache crashes and won't start due to an out of space error related to mod_rewrite.  The following command frees the stuck resources and allows apache to be restarted.

```bash
$ ipcs -s | grep admin | perl -e 'while (<STDIN>) { @a=split(/\s+/); print `ipcrm sem $a[1]`}' 
$ apache-ssl -f /etc/frontview/apache/httpd.conf -k start 
```

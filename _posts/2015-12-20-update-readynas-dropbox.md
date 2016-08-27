---
layout: post
title: "Update ReadyNAS dropbox installation"
date: 2015-12-20
last_modified: 2016-08-11
categories:  dropbox readynas
---

Use su to become the user who is running dropbox and download/install the newest version.

```bash
su -s /bin/bash username
cd ~ && wget --no-check-certificate -O - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -
```

Then you can check the daemon using

```bash
~/.dropbox-dist/dropboxd
```

Also, may need to upgrade the the `DAEMON` environment variable in `/etc/init.d/dropbox`

More information at [https://www.dropbox.com/install?os=lnx][1]

[1]: https://www.dropbox.com/install?os=lnx


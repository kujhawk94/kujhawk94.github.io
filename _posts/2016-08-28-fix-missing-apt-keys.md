---
layout: post
title: Missing GPG keys in apt-get
date: 2016-08-28
categories: gpg apt
---
Fixing missing keys in apt-get is a two step process.  First, download the missing key from an appropriate key server.  Second, tell `apt` about the new key it can use.  

So, for example to install the key CBF8D6FD518E17E1, download the key from `pgpgkeys.mit.edu`.  A second key server to try is `subkeys.pgp.net`.

```
gpg --keyserver pgpkeys.mit.edu --recv CBF8D6FD518E17E1
```

Tell `apt` about the new key with the `apt-key add` command.

```
gpg --export --armor CBF8D6FD518E17E1 | apt-key add -
```

> Ref: [Fix Missing GPG Key For apt-get][1]

[1]: http://ram.kossboss.com/fix-missing-gpg-key-apt-get/
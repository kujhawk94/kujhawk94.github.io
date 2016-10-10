---
title: New encryption key
date: 2016-10-09
layout: post
---
The old key is

pub   1024D/A4ABD822 2008-12-11 [expires: 2018-01-02]
      Key fingerprint = EF50 0DE2 E446 D505 725A  0700 B4FA B072 A4AB D822

And the new key is

pub   4096R/40F472C4 2016-10-09
      Key fingerprint = 5058 E5B6 FF3F 0FEE 7A17  CE1A DAAB 577A 40F4 72C4

If you are satisfied that you've got the right key, and the UIDs match
what you expect, I'd appreciate it if you would sign my key:

  gpg --sign-key 40F472C4

Lastly, if you could upload these signatures, I would appreciate it.
Please could you just upload the signatures to a public keyserver directly:

  gpg --keyserver pgp.mit.edu --send-key 40F472C4

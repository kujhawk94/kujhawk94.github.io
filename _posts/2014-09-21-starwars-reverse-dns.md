---
layout: post
title:  "Star Wars Reverse DNS"
date:   2014-09-21
categories: geek
---
My first exposure to this was via a FB post from [Tony Gabel][1].  A bit of [Command Line Magic][2] makes the process almost painless, but removes the magic of finding the hidden host names via traceroute.  As noted in Tony's post, it takes a certain amount of network geekiness to appreciate the way the lines appear.

NB: A similar technique can be used starting at obiwan.scrye.net  RR

```shell
( seq 1 8 200 ; seq 6 8 200 ) | sort -n | xargs -I{} -n 1 dig +short -x 206.214.251.{}
```

[1]:http://www.fhsu.edu/management/gabel/
[2]:https://twitter.com/climagic/status/513705570923466752
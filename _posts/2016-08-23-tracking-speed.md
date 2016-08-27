---
layout: post
date: 2016-08-23
title:  CLI adjustment of Mac mouse tracking speed
---
Adjustments to the mouse tracking speed are availabe via the command line and allow for settings not accessbile via system preference pane.

Read the current speed with 

```plaintext
defaults read -g com.apple.mouse.scaling
```

The maximum speed available via system preferences is 3.  Writing a new value via the command line allows higher tracking speeds.  NB, restarting the mac may be required to allow the changes to take effect.

```plaintext
defaults write -g com.apple.mouse.scaling 4
```


> Ref: [Speed up Mouse Tracking on Mac OS X][1] at tylernichols.com

[1]: http://www.tylernichols.com/apple/speed-up-mouse-tracking-on-mac-os-x
---
layout: post
title: Hiding username from MacOS login screen
date: 2018-01-12
---
A simple switch with the Directory Service command line utility hides the account with short name `ACCOUNTNAME` from the login screen ([Ref][1]).

    sudo dscl . create /Users/ACCOUNTNAME IsHidden 1
    
[1]:http://osxdaily.com/2015/02/01/hide-specific-user-account-mac-os-x/
    
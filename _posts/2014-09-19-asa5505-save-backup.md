---
layout: post
title:  "ASA 5505 - Save backup of running configuration file via tftp"
date:   2014-09-19
last_modified:  2017-07-26
categories: cisco
redirect_from: "/archives/173"
---

Before running an upgrade to the ASA operating system, I wanted to make a backup of the current running configuration in case of catastrophe.  Ideally, that backup would exist somewhere not on the ASA device itself which involves getting something like *write net* to work.  Fortunately, the process is not difficult but posting a few notes will hopefully save me from reinventing the wheel in a couple of years when I need to do this again.  These steps are based on the [a page from Dario Garavini](http://www.dariogaravini.com/update-cisco-ios-tftp-os-x-mavericks/) and the [Cisco support site](http://www.cisco.com/c/en/us/support/docs/security/pix-500-series-security-appliances/70771-backup-restore-pix-configure.html).

## Information regarding tftpd on debian (2017-07-26)
A few more notes which would have saved me ten or fifteen minutes today when attempting the same operation with a debian tftpd server.  First installation is simple enough with
```
$ sudo apt-get install tftpd 
```
By default, tftpd is configured in `/etc/inetd.conf` to use the directory `/srv/tftp`.  But that directory is not created during installation.  If necessary to restart `inetd`, [the command is](http://imranasghar.blogspot.com/2008/09/how-to-setup-tftp-server-on-debian.html)
```
$ sudo /etc/init.d/openbsd-inetd restart
```

Implied in the text below but not explicitly stated is before any file can be saved to the via `tftp` a corresponding filename needs to already exist on the server.  The `tftp-server` command below sets the filename to `asaconfig` so a simple `touch asaconfig` in `/srv/tftp` will prepare the server for the incoming file.  Failure to execute something like the touch command will result in the following:  *Error code 2: Access violation*

## Mac OS X TFTP

While Mac OS X has its own TFTP server, a [GUI interface](http://ww2.unime.it/flr/tftpserver/) makes the experience much more pleasant.

Once the TFTP manager is loaded, you can select the root directory for the TFTP server.  The GUI does a nice job of alerting you to permissions errors for the directory, etc.  Finally, make sure to start the TFTP service.

![][1]

[1]: /images/asa-5505---save-running-configuration-file-to-network/mac-os-x-tftp.png

## Create a new empty file for uploads

Click on the Create File button

![][2]

[2]: /images/asa-5505---save-running-configuration-file-to-network/create-a-new-empty-file-for-uploads.png

### Enter the desired filename and click OK

Even though nothing appears different in the application window, the empty file is ready to receive data from the ASA.

![][3]

[3]: /images/asa-5505---save-running-configuration-file-to-network/enter-the-desired-filename-and-click-ok.png

## ASA CLI commands to write the config file

Set the *tftp-server* information and then issue the *write net* command.

![][4]

[4]: /images/asa-5505---save-running-configuration-file-to-network/asa-cli-commands-to-write-the-config-file.png

## Note the new file in the TFTP server application

The file is now visible and ready to copy elsewhere for safe keeping.

![][5]

[5]: /images/asa-5505---save-running-configuration-file-to-network/note-the-new-file-in-the-tftp-server-application.png
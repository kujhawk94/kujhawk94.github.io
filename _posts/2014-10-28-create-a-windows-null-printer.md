---
layout: post
title: Create a Windows null printer
date: 2014-10-28
redirect_from: "/notes/create-a-windows-null-printer"
categories: e-mds
---

Basically a null printer redirects all output to the Windows [null device](https://en.wikipedia.org/wiki/Null_device), NUL, a remnant from the days of MS-DOS (you still sometimes see references to other such devices:  PRN, LPT1, COM1, etc.)  Sending anything to the null device discards all data written to it, but reporte the write operation succeeded.

## Devices and Printers

Open the **Devices and Printers** panel and select **Add a printer**.

![][1]

[1]: /images/create-a-windows-null-printer/devices-and-printers.png

## Add Printer (Step 1)

Select **Add a local or network printer as an administrator**.  I don't know if this screen always appears – it may only be on Windows Server depending on security settings?

![][2]

[2]: /images/create-a-windows-null-printer/add-printer--step-1-.png

## Add Printer (Step 2)

Select **Add a local printer**

![][3]

[3]: /images/create-a-windows-null-printer/add-printer--step-2-.png

## Add Printer (Step 3)

Select **Create a new port:** and select the type of port to be **Local Port**.  Then click **Next**.

![][4]

[4]: /images/create-a-windows-null-printer/add-printer--step-3-.png

### Port Name

Type in the port name ***nul*** and click **OK**.

![][5]

[5]: /images/create-a-windows-null-printer/port-name.png

## Add Printer (Step 4)

The actual printer driver does not matter, but it might as well be a simple one.  I select the **Generic / Text Only** driver.  Then click **Next**.

![][6]

[6]: /images/create-a-windows-null-printer/add-printer--step-4-.png

## Add Printer (Step 5)

Choose an appropriate name for the printer and click **Next**.

![][7]

[7]: /images/create-a-windows-null-printer/add-printer--step-5-.png

## Add Printer (Step 6)

If you share the printer, then everyone can print nowhere!  Click **Next**.

![][8]

[8]: /images/create-a-windows-null-printer/add-printer--step-6-.png

## Add Printer (Step 7)

You can print a test page, but don't expect much to happen.  Click **Finish**.

![][9]

[9]: /images/create-a-windows-null-printer/add-printer--step-7-.png
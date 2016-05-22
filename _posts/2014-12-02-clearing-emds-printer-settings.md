---
layout: post
title:  "Clearing e-MDs printer settings for all workstations"
date:   2014-12-02
categories: e-mds
redirect_from: "/notes/clearing-e-mds-printer-settings-for-all-workstations"
---

From time to time, e-MDs Solution Series printer settings may require a reset.  The following steps show how to locate the information in TopsData and reset a user's printer preferences.

## Find the user's printer preferences in TopsData

Solution series printing options reside in the User_Preference table.  The following query allows you to nd the correct record given a particular username.  In this instance, I am finding the settings for the er with login `cbrull`.

    SELECT *
    FROM USER_Preference AS up
    INNER JOIN SECR_Login AS lo ON up.Login_ID = lo.Login_ID WHERE 
    up.Preference_Category = 'TOPSCHART PRINTING'
    AND lo.login_name = 'cbrull'

## Query results

The actual settings are contained in the column `Preference_StringList`.  

![][1]

## Contents of `Preference_StringList`

Copy and pasting the contents of field `Preference_StringList` so we can see all of the text.  If you inspect these records for live users, you'll see a line of text for each configuration item for each workstation.

![][2]

## Printing tab in Solution Series

To verify the settings match those presented in Solution Series.

![][3]

## Clearing the preferences

Given that each record has a field `Preference_RecordState` it is probably safest just to set that to 1 to delete the record.

## Locate the record identifier

Copy the value of `Preference_ID` for use in the update query.

![][4]

## The update query

By setting `Preference_RecordState` to 1, the settings will be cleared.  Change the highlighted value to the id copied in the previous step.

    UPDATE User_Preference 
    SET Preference_RecordState = 1 
    WHERE Preference_ID = 'ZZZZZ0075T'

## Checking Solution Series

The settings are cleared.  NB: Solution Series does not appear to recheck the database preferences except on launch, so you'll have to quit and restart Solution Series.

![][5]

[1]:/images/clearing-emds-printer-settings/query-results.png
[2]:/images/clearing-emds-printer-settings/contents-of-preference-stringlist.png
[3]:/images/clearing-emds-printer-settings/printing-tab-in-solution-series.png
[4]:/images/clearing-emds-printer-settings/locate-the-record-identifier.png
[5]:/images/clearing-emds-printer-settings/checking-solution-series.png
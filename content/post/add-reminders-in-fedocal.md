+++
author = "Ratnadeep Debnath"
categories = ["Fedora", "fedocal", "rtnpro"]
date = 2015-03-31T17:43:09Z
description = ""
draft = false
slug = "add-reminders-in-fedocal"
tags = ["Fedora", "fedocal", "rtnpro"]
title = "Meeting reminders in fedocal"

+++


fedocal 0.13 release makes it possible for anyone to download iCal file for a meeting and brings support for client side notifications.

You can do this in the following ways:

- Click on a meeting in fedocal
- In the meeting modal/page, click on the `iCal export` link to download iCal file for that particular meeting.
- You can also export iCal file for the meeting with reminder info in by checking the reminder checkbox beside the iCal export link, and then clicking on the ``iCal export`` link.
![](/content/images/2015/03/fedocal_meeting_ical_export.png)

Once you have downloaded the iCal file for the meeting, you can import the iCal file using `Evolution` in Gnome 3.
![](/content/images/2015/03/fedocal_ical_import-1.png)

If you have added your Google account in Gnome 3's online accounts and have the calendar service enabled, you'll also have the option to import the iCal data directly to Google calendar.
![](/content/images/2015/03/fedocal_ical_import_to_google-1.png)

Once the iCal data is imported to Google calendar using Evolution, you can find your event in Google Calendar, too.
![](/content/images/2015/03/fedocal_ical_imported_to_google.png)

This makes it very easy to subscribe to a specific meeting in fedocal, without dragging the whole calendar, and get notifications in your client directly (google calendar(Android), evolution (desktop), etc.)


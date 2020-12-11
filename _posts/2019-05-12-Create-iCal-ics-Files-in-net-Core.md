---
layout: post
title: Creating iCal ics Files in C# ASP.NET Core
tags: [iCal, c#, .net, calendar, iCal.NET, iCalendar]
comments: true
---

In this post I will show you how to create iCal files using [iCal.NET](https://github.com/rianjs/ical.net) library.

[![logo](https://raw.githubusercontent.com/rianjs/ical.net/master/logo.png)]

iCal.NET is an iCalendar (RFC 5545) class library for .NET aimed at providing RFC 5545 compliance, while providing full compatibility with popular calendaring applications and libraries.

# Calendar (.ics) File Structure

iCalendar, or ICS, is a standardized format for storing and transmitting calendar data, including scheduled events and "to-do" lists.

Every calendar file stored on a CalDAV server contains a single iCalendar object with a single event or to-do defined inside it. Every event/to-do object consists of one or more event or to-do components. It also typically contains one or more time zone component.

First of all, this is the basic format for an iCal:

```
BEGIN:VCALENDAR
PRODID:-//LEEDS MUSIC SCENE//EN
VERSION:2.0
METHOD:PUBLISH
BEGIN:VEVENT
SUMMARY:BAND @ VENUE
PRIORITY:0
CATEGORIES:GIG
CLASS:PUBLIC
DTSTART:STARTTIME
DTEND:ENDTIME
URL:LINK TO LMS GIG PAGE
DESCRIPTION:FULL BAND LIST
LOCATION:VENUE
END:VEVENT
END:VCALENDAR
```

Source: [This PasteBin](https://pastebin.com/QZ6enx2Z)


# Using iCal.NET

Install iCal.NET via [NuGet iCal.NET](https://www.nuget.org/packages/Ical.Net) in your project.

In the code below I create an email message and attach the iCal to one.

Everything else in the code below is pretty self-explanatory.

For event detail create `CalendarNotificationModel` class with all necessary info.

Then in `CreateCalendarEventAsync` method create `CalendarEvent` model with orginizer info (ORGANIZER;CN=Your app name:mailto:email@company.com).

Put it into a memory stream and finally attach it to the email message. 

Sending email in .NET Core with [FluentEmail](https://www.nuget.org/packages/FluentEmail.Core/) and [SendGrid](https://sendgrid.com/). For this one add `EmailProviderService`.

<script src="https://gist.github.com/Stayrony/1f885bddd7da88f414f6205a138516a6.js"></script>

# Results

The following screenshots show results:

*The Gmail experience:*

![screenshot](/images/iCal/Gmail.jpg "The Gmail experience")

*The Outlook experience:*

![screenshot](/images/iCal/Outlook.jpg "The Outlook experience")

*The Mail Mac OS experience:*

![screenshot](/images/iCal/MailMacOS.jpg "The Mail Mac OS experience")

# References
1. [Calendar (.ics) File Structure](https://www.webdavsystem.com/server/creating_caldav_carddav/calendar_ics_file_structure/)
2. [Create iCal ics Files in C# ASP.NET MVC – Several Methods](https://esausilva.com/2016/11/17/create-ical-ics-files-in-c-asp-net-mvc-several-methods/)
3. [Creating iCal files in ASP.NET MVC and C#](http://www.nodeomega.com/blog/2015/04/19/creating-ical-files-in-aspnet-mvc-and-c)

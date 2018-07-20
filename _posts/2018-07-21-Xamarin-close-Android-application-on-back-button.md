---
layout: post
title: Xamarin Close Android app on back button 
tags: [Xamarin, Xamarin.Android, BackButton, c#, .net]
comments: true
---
Some time in your application, it is needed to prompt user before exit from application. In this article, we will see how you can prompt user on pressing back button with a dialog, questioning whether or not the user wishes to exit the application.
So, in this article we are going to learn how to prevent user to exit from application without giving response.

# Getting it

Go to your Droid project and open MainActivity.cs file and add below code to your onBackPressed() method.

<script src="https://gist.github.com/Stayrony/db39613bdaf7ac49c382395fba21228a.js"></script>

AlertDialog described [here](https://medium.com/@dannyc/xamarin-android-await-alertdialog-builder-42d9c861e37a).

As a result of clicks back button user will be asked for confirmation for exit:

![screenshot](/images/HardwareBackButton/HardwareBackButton_nexus5x-portrait.png "Hardware BackButton")

---
layout: post
title: Continuous Integration for iOS with DevOps
tags: [Xamarin.Forms, CI, CI/DI, DevOps, build, vsts]
comments: true
---

## Creating a DevOps Team Project

Sign in for a DevOps account and create a new Team Project for your Xamarin projects. If you do not have access to creating a new project ask your admin to create one.

## Connecting to Code

Imagine that our project will be name Entree.
![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/1.png "Organization projects")

Go to project details and you will see all available info:

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/2.png "")

Go to Pipelines → Builds

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/3.png "")

And create the new build pipeline. If the project has not previously configured builds, then your image may differ from this one:

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/4.png "")

Select a source code for builds. Authorize Bitbucket connection if needed:

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/5.png "")

Select the repository and build branch:

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/6.png "")

## Creating the Build Definition

To set up our Xamarin iOS build we use the available template in VSTS because it contains all the necessary build steps to create an IPA file which is Apple's equivalent of the APK file in Android. Time to use that template and create our build definition!

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/7.png "")

The tasks displayed below will be the ones you'll end up with and once again some of them are greyed out (disabled). Leave them in or remove them by right clicking and selecting **Remove Selected Task(s)**.

Depend on the environment rename name of the build.

For DEV - **D**_Entree-Xamarin.iOS-CI

For STAGE - **S**_Entree-Xamarin.iOS-CI

For RELEASE - **R**_Entree-Xamarin.iOS-CI

Select **Hosted macOS** for iOS build.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/8.png "")

### Signing & Provisioning

Enable Install an Apple certificate and Install an Apple provisioning profile steps.

This is where you put your P12 signing key, the password of your signing key and a provisioning profile containing e.g. your test devices. Don't know where to get these? After selecting your key and profile you can put in a variable for the password. Since we don't want to have that in our build definition in plain text we put in $(P12Password) and set this at a later point.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/9.png "")

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/10.png "")

All other steps, including restoring NuGet packages and use it, building the iOS project, and packaging the app can remain.

### Build Xamarin.iOS solution **/*.sln

Uncheck _Build for iOS Simulator._

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/11.png "")

### Next step distribute app builds to testers and users via App Center.

Enable this task.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/12.png "")

Configure App Center service connection. You need API token for connection, ask your admin for it.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/13.png "")

### After that, you need app slug.

The app slug is in the format of **{username}/{app_identifier}**. To locate **{username}** and **{app_identifier}** for an app, click on its name from [https://appcenter.ms/apps](https://appcenter.ms/apps), and the resulting URL is in the format of [https://appcenter.ms/users/](https://appcenter.ms/users/) **{username}**/apps/ **{app_identifier}**. If you are using orgs, the app slug is of the format **{orgname}/{app_identifier}**.

Fill in release notes. For all app we define notes like below:

`**Release notes for build** $(Build.BuildNumber)`

`**The latest version control change is** - $(Build.SourceVersion)&#xA0;`<br>
`**Branch** - $(Build.SourceBranch)&#xA0;`<br>
`**Commit** - $(Build.SourceVersionMessage`

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/14.png "")

**Save your work.**

### Next step Slack Notification.

Go to Task groups and select Import. Upload Slack task groups.

You can download this file here - [Slack Notification.json](https://headworks.atlassian.net/wiki/download/attachments/627605511/Slack%20Notification.json?version=1&modificationDate=1554315792037&cacheVersion=1&api=v2)

Or export from existing builds.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/15.png "")

After importing JSON file you should see something like this:

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/16.png "")

Save this groups task.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/17.png "")

Go back to our build and click to Add a task to Agent:

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/18.png "")

In the Build tab, you should find previously created SlackNotification task. Select and Add this to job:

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/19.png "")

Click on one and you should see below settings, in the next step we define variables (Slack.ApiToken and Slack.Channel):

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/20.png "")

## Specifying variables and triggers

In the previous step, we created some variables containing passwords related to our keystore. These variables need to be declared somewhere so let�s switch to the **Variables** tab and add them. You can use the lock icon to make sure that people with build definition editing permissions can�t simply read these values. Make sure you still have these passwords saved elsewhere because you won�t be able to read them either after clicking the lock icon. Create all the necessary variables for the build. And save.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/21.png "")

### Enables triggers.

Now we get to the part where we need to set a trigger for our build. Do we want to queue a build manually every time? Of course not! We want to setup continuous integration so let�s start by enabled that in the Triggers tab of the build definition. We can then define one or more Branch Filters. These filters define which branch to monitor for changes and tell VSTS to trigger a build as soon as a commit/push to these remote branches happens. An alternative to these Branch Filters are Path Filters which let you specify one or more specific paths in your project hierarchy to monitor. Both of these functions can be used in conjunction with one another.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/22.png "")

### Set up options:

Change build number format to

`$(TeamProject)_$(SourceBranchName)_$(Date:yyyyMMdd)$(Rev:.rr)`

and save your work.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/23.png "")

And that's it! We created a Xamarin iOS continuous integration pipeline! All that is left it to either manually queue a build to check if it works or to commit to the branch you configured. If all goes well you should see a load of green checkmarks.

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/24.png "")

Check artifacts:

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/25.png "")

And Slack notifications:

![screenshot](/images/ContinuousIntegrationForXamarinFormsiOSWithDevOps/26.png "")

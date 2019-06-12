---
layout: post
title: Create different build configuration to control runtime settings for different environments
tags: [Architecture, Xamarin, NET, build, conditional compilation, conditional methods]
comments: true
---

In general enterprise mobile applications have more than one deployment scenarios, each of which requires the same code base, but different application settings.

The main three environments are development, stage, and production. You can read more info about environments [here](https://dev.to/flippedcoding/difference-between-development-stage-and-production-d0p). We can define Conditional Compilation symbols, and load different application settings depending upon which build configuration is running.

# Set up build configurations

Imagine that our project will name Tritan.

Go to Options → Configuration on solution Xamarin project:

![screenshot](/images/BuildConfigurations/1.png "Configuration")

We will create configurations for Android, iPhone, and iPhoneSimulator platforms.

Create a new Build Configuration named DEV for Any CPU platform, and copy settings from the default Release configuration. Also set `Create configuration for all solution items` which create a config for all projects:

![screenshot](/images/BuildConfigurations/2.png "Create configuration for all solution items")

Create the same DEV configuration for iPhone platform, and copy settings from the default Release|iPhone configuration:

![screenshot](/images/BuildConfigurations/3.png "DEV configuration for iPhone platform")

Create the same DEV configuration for iPhoneSimulator platform, and copy settings from the default Release|iPhoneSimulator configuration:

![screenshot](/images/BuildConfigurations/4.png "DEV configuration for iPhoneSimulator platform")

Then repeat these steps to create STAGE configurations for all platforms:

![screenshot](/images/BuildConfigurations/5.png "STAGE configurations")

# Set configuration mappings 

Go to the next tab and set Configuration Mappings settings:

![screenshot](/images/BuildConfigurations/6.png "Configuration Mappings settings")

Map config according to video. 

If we build Tritan.Android we don't need to build Tritan.iOS project. And vice versa, for Tritan.iOS we don't need to build Tritan.Android project.

<iframe width="560" height="315" src="https://www.youtube.com/embed/BIwGQ7HZB7Y" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Edit config for Tritan.Android

Now we should remove iPhone and iPhoneSimulator configurations from the Tritan.Android project. 

Go to Options → Configuration on Tritan.Android project:

![screenshot](/images/BuildConfigurations/7.png "Configuration on Tritan.Android")

And remove above-mentioned config:

![screenshot](/images/BuildConfigurations/8.png "Remove above-mentioned config")

**Note!**

Remove only for Android project (uncheck `Delete also configuration in solution items`):

![screenshot](/images/BuildConfigurations/9.png "Delete also configuration in solution items for Android")

It should look like this:

![screenshot](/images/BuildConfigurations/10.png "Android Config")

# Edit config for Tritan.iOS
The same for Tritan.iOS project we need to remove Android configuration. 

Go to Options → Configuration on Tritan.iOS project:

![screenshot](/images/BuildConfigurations/11.png "Configuration on Tritan.iOS project")

And remove above-mentioned config:

![screenshot](/images/BuildConfigurations/12.png "Remove config")

**Note!**

Remove only for Android project (uncheck `Delete also configuration in solution items`):

![screenshot](/images/BuildConfigurations/13.png "Delete also configuration in solution items for iOS")

It should look like this:

![screenshot](/images/BuildConfigurations/14.png "Configuration on Tritan.Android project")

# Define conditional compilation symbols for each build configuration
Now we need to define Conditional Compilation symbols for each build configuration. Open the `Project Properties` window from the solution explorer on the Core (Tritan) project, and select the `Compiler` tab. In the dropdown list of available configurations, you will now see the default Debug and Release options, as well as your newly defined DEV and STAGE configurations. First, we will select the DEV configuration, and add a `Conditional Compilation symbol` in the space provided:

![screenshot](/images/BuildConfigurations/15.png "Define conditional compilation symbols for Tritan.Android project")

The same for iPhone and iPhoneSimulator platforms:

Save config by pressing `OK`.

Add `STAGE` for Stage configuration for all platforms.

After that, we need to do the same for Tritan.iOS and Tritan.Android projects. 

Android dev:

![screenshot](/images/BuildConfigurations/16.png "Android dev")

iOS dev (for iPhone and iPhomeSimulator platforms):

![screenshot](/images/BuildConfigurations/17.png "Define conditional compilation symbols for Dev Tritan.iOS project")

iOS stage (for iPhone and iPhomeSimulator platforms):

![screenshot](/images/BuildConfigurations/18.png "Define conditional compilation symbols for Stage Tritan.iOS")

Save config by pressing `OK`.

# Define general application settings

Let`s try our configs for some environment settings in Constant class. Define for all environments different URL and select, for example, DEV | iPhone configuration:

![screenshot](/images/BuildConfigurations/19.png "Result")

And voila we can see that active config is DEV (all others are disabled). You may try to check another config.

# References
1. [Visual Studio: Use Conditional Compilation to Control Runtime Settings for Different Deployment Scenarios](http://johnatten.com/2012/08/18/visual-studio-use-conditional-compilation-to-control-runtime-settings-for-different-deployment-scenarios/)

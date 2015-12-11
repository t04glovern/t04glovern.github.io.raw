---
layout: post
title: "Learning ASP.NET (Part 1)"
date: "2015-12-11"
tags:
 - asp.net
 - windows
---

## Introduction to the ASP.NET Runtime

`ASP.NET` is an open-source server-side web application framework designed for web development to produce dynamic webpages. That's what [Wikipedia](https://en.wikipedia.org/wiki/ASP.NET) says about it anyway... I personally had no clue what it was until I recently began [taking a course](https://app.pluralsight.com/library/courses/aspdotnet-5-ef7-bootstrap-angular-web-app) that was available as part of the free 6 months worth of [Pluralsight](https://app.pluralsight.com) I picked up from the [Visual Studio Dev Essentials pack](https://myprodscussu1.app.vssubscriptions.visualstudio.com/Dashboard).

## Three Frameworks

There are three frameworks available, each of them seamlessly integrate with one and other and provide different levels of functionality.

![ASP.NET Hosting]({{ site.url }}/images/posts/asp.net-hosting.png)

`.NET 4.6` is the .NET we've always known. It was built for the windows platform so it didn't natively function on other systems. This is where `Mono` came in.

`Mono` is a community driven project that allows .NET code to run on OSX/Linux systems. It is still widely used and very popular.

`.NET Core` is the most recent edition to the .NET family. It's open source like Mono but supports both Windows, OSX and Linux platforms.

> **CoreCLR** is a subset of the .NET Framework.
{: .note}

## Installing ASP.NET

1. Start by first installing [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) if you haven't already got it on your system.

> The install process can be quite lengthy, maybe go get 5-6 cups of coffee while you wait.
{: .note}

2. Download and install `ASP.NET 5` from [here](https://go.microsoft.com/fwlink/?LinkId=627627).

3. You will now have access to the `DNVM` cmdlet. Running the following command with display a full list of commands you have access to.

{% highlight bat %}
C:\Users\Nathan>dnvm
   ___  _  ___   ____  ___
  / _ \/ |/ / | / /  |/  /
 / // /    /| |/ / /|_/ /
/____/_/|_/ |___/_/  /_/
.NET Version Manager v1.0.0-rc1-15540
By Microsoft Open Technologies, Inc.
usage: dnvm <command> [<arguments...>]

Current feed settings:
Default Stable: https://www.nuget.org/api/v2
Default Unstable: https://www.myget.org/F/aspnetvnext/api/v2
Current Stable Override: <none>
Current Unstable Override: <none>

    To use override feeds, set DNX_FEED and DNX_UNSTABLE_FEED environment keys respectively

commands:
    alias           Lists and manages aliases
    exec            Executes the specified command in a sub-shell where the PATH has been augmented to include the specified DNX
    help            Displays a list of commands, and help for specific commands
    install         Installs a version of the runtime
    list            Lists available runtimes
    run             Locates the dnx.exe for the specified version or alias and executes it, providing the remaining arguments to dnx.exe
    setup           Installs the version manager into your User profile directory
    uninstall       Uninstalls a version of the runtime
    update-self     Updates DNVM to the latest version.
    upgrade         Installs the latest version of the runtime and reassigns the specified alias to point at it
    use             Adds a runtime to the PATH environment variable for your current shell
    version         Displays the DNVM version.
{% endhighlight bat %}

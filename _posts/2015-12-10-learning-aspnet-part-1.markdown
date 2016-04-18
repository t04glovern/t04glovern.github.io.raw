---
layout: post
title: "Learning ASP.NET - Installing ASP.NET Runtime (Part 1)"
date: "2015-12-10"
comments: true
description: We run through the basics of the ASP.NET framework including installing and configuring the platform
share: true
author: nathan
tags:
 - asp.net
 - windows
---

## Introduction to the ASP.NET Runtime

***

`ASP.NET` is an open-source server-side web application framework designed for web development to produce dynamic webpages. That's what [Wikipedia](https://en.wikipedia.org/wiki/ASP.NET) says about it anyway... I personally had no clue what it was until I recently began [taking a course](https://app.pluralsight.com/library/courses/aspdotnet-5-ef7-bootstrap-angular-web-app) that was available as part of the free 6 months worth of [Pluralsight](https://app.pluralsight.com) I picked up from the [Visual Studio Dev Essentials pack](https://myprodscussu1.app.vssubscriptions.visualstudio.com/Dashboard).

***

## Three Frameworks

***

There are three frameworks available, each of them seamlessly integrate with one and other and provide different levels of functionality.

![ASP.NET Hosting]({{ site.url }}/images/posts/2015.12.10/asp.net-hosting.png)

`.NET 4.6` is the .NET we've always known. It was built for the windows platform so it didn't natively function on other systems. This is where Mono came in.

`Mono` is a community driven project that allows .NET code to run on OSX/Linux systems. It is still widely used and very popular.

`.NET Core` is the most recent edition to the .NET family. It's open source like Mono but supports both Windows, OSX and Linux platforms.

* **CoreCLR** is a subset of the .NET Framework.

***

## Installing ASP.NET

***

1. Start by first installing [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) if you haven't already got it on your system.
* The install process can be quite lengthy, maybe go get 5-6 cups of coffee while you wait.

2. Download and install `ASP.NET 5` from [here](https://go.microsoft.com/fwlink/?LinkId=627627).

3. You will now have access to the `DNVM` cmdlet. Running the following command will display a full list of commands you have access to.

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

***

## Setting up

***

Running the command `dnvm list` from command line will result in a list of the current versions of ASP.NET 5 you currently have installed on your system

{% highlight bat %}
C:\Users\Nathan>dnvm list

Active Version           Runtime Architecture OperatingSystem Alias
------ -------           ------- ------------ --------------- -----
  *    1.0.0-rc1-update1 clr     x64          win             default
       1.0.0-rc1-update1 clr     x86          win
       1.0.0-rc1-update1 coreclr x64          win
       1.0.0-rc1-update1 coreclr x86          win
{% endhighlight bat %}

These runtimes are stored in your user directory, and you can prove this by listing the files in `.dnx\runtimes`. Run the following to show a list of installed runtimes under your home directory.

{% highlight bat %}
C:\Users\Nathan>dir .dnx\runtimes
 Volume in drive C has no label.
 Volume Serial Number is 2A20-B603

 Directory of C:\Users\Nathan\.dnx\runtimes

11/12/2015  12:02 AM    <DIR>          .
11/12/2015  12:02 AM    <DIR>          ..
10/12/2015  11:56 PM    <DIR>          dnx-clr-win-x64.1.0.0-rc1-update1
10/12/2015  11:45 PM    <DIR>          dnx-clr-win-x86.1.0.0-rc1-update1
11/12/2015  12:01 AM    <DIR>          dnx-coreclr-win-x64.1.0.0-rc1-update1
11/12/2015  12:02 AM    <DIR>          dnx-coreclr-win-x86.1.0.0-rc1-update1
               0 File(s)              0 bytes
               6 Dir(s)  44,004,626,432 bytes free
{% endhighlight bat %}

You can also run `dnvm upgrade` in order to pull down the most recent versions of the ASP.NET 5 runtime. If there is a more recent runtime available it will be downloaded and set as your default runtime.

{% highlight bat %}
C:\Users\Nathan>dnvm upgrade
Determining latest version
'dnx-clr-win-x86.1.0.0-rc1-update1' is already installed in C:\Users\Nathan\.dnx\runtimes\dnx-clr-win-x86.1.0.0-rc1-update1.
Adding C:\Users\Nathan\.dnx\runtimes\dnx-clr-win-x86.1.0.0-rc1-update1\bin to process PATH
Adding C:\Users\Nathan\.dnx\runtimes\dnx-clr-win-x86.1.0.0-rc1-update1\bin to user PATH
Updating alias 'default' to 'dnx-clr-win-x86.1.0.0-rc1-update1'
{% endhighlight bat %}

You can also manually pull down other versions using the following in order to support different architectures.

{% highlight bat %}
dnvm install 1.0.0-rc1-update1 -arch x64
{% endhighlight bat %}

You might also want the `coreclr` versions, in which case you can run the following to pull down the respective version. Do the same for the x86 version.

{% highlight bat %}
dnvm install 1.0.0-rc1-update1 -r coreclr
dnvm install 1.0.0-rc1-update1 -r coreclr -arch x86
{% endhighlight bat %}

With a full suite of runtimes installed, you can now use the following command to switch between runtimes.

{% highlight bat %}
dnvm use 1.0.0-rc1-update1 -r clr -arch x64 -p
{% endhighlight bat %}

Note that the `-p` makes the path persistent so you won't need to re-run the command each time you open a new terminal

Running `dnx --version` will show you the runtime currently active

{% highlight bat %}
C:\Users\Nathan>dnx --version
Microsoft .NET Execution environment
 Version:      1.0.0-rc1-16231
 Type:         Clr
 Architecture: x86
 OS Name:      Windows
 OS Version:   10.0
 Runtime Id:   win10-x86
{% endhighlight bat %}

***

## Hello World

***

At this stage we are ready to use the runtime we just installed. In order to do this you'll need to create two new files in a working directory with the following contents

`project.json`

Specifies the framework we want to use. can also contain other project based traits (you'll see this in up-comming tutorials)
{% highlight js %}
{
  "frameworks":
  {
    "dnx451":{}
  }
}
{% endhighlight js %}

`program.cs`

Contains straight `C#` code.
{% highlight csharp %}
using System;

public class Program
{
  public void Main()
  {
    Console.WriteLine("Hello World");
  }
}
{% endhighlight csharp %}

And that's it!. You can run your Hello World using the following syntax:

{% highlight bat %}
C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\helloWorld>dnx run program.cs
Hello World
{% endhighlight bat %}

***

## Summary

***

As someone new to the .NET scene I was astounded by how simple it was to get started with ASP.NET and I can't wait to get my fingers into some more difficult challenges. I would highly recommend checking out the [course](https://app.pluralsight.com/library/courses/aspdotnet-5-ef7-bootstrap-angular-web-app) I wrote this for; and even follow along with me as I make my way through the several hours worth of [FREE](https://myprodscussu1.app.vssubscriptions.visualstudio.com/Dashboard) content.

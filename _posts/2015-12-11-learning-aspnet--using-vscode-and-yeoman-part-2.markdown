---
layout: post
title: "Learning ASP.NET - Using VScode and Yeoman (Part 2)"
date: "2015-12-11"
comments: true
description: Using Yeoman to deploy a ASP.NET project template, we generate our first working page
share: true
author: nathan
tags:
 - asp.net
 - windows
 - yeoman
 - vscode
---

## Installing and Using Yeoman

***

`ASP.NET` uses a tool called yeoman to assist in the development of applications for the web by generating the base project file structure and configurations so you can get to work quickly; rather than fiddling around with the same things every time you need to create a new project.

You can install `Yeoman` using the following command:

{% highlight bat %}
C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\vscode>npm install yo -g
C:\Users\Nathan\AppData\Roaming\npm\yo -> C:\Users\Nathan\AppData\Roaming\npm\node_modules\yo\lib\cli.js

> yo@1.5.0 postinstall C:\Users\Nathan\AppData\Roaming\npm\node_modules\yo
> yodoctor


Yeoman Doctor
Running sanity checks on your system

√ Global configuration file is valid
√ NODE_PATH matches the npm root
× Node.js version

Your Node.js version is outdated.
Upgrade to the latest version: https://nodejs.org

√ No .bowerrc file in home directory
√ No .yo-rc.json file in home directory
× npm version

Your npm version is outdated.

Upgrade to the latest version by running:
npm install -g npm
{% endhighlight bat %}

Ensure that you used the `-g` flag to tell the system you want Yeoman available globally.

Next we can install the ASP.NET templates using the following command: (include the -g again for global)

{% highlight bat %}
C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\vscode>npm install generator-aspnet -g
{% endhighlight bat %}

You can now run the 'yo' cmdlet to view the current installed generators on your system:

{% highlight bat %}
C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\vscode>yo
? 'Allo Nathan! What would you like to do? (Use arrow keys)
  Run a generator
> Aspnet
  ──────────────
  Update your generators
  Install a generator
  Find some help
  Get me out of here!
  ──────────────
{% endhighlight bat %}

You can see we have Aspnet installed and listed. Run this generator by using the following cmdlet:

{% highlight bat %}
C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\vscode>yo aspnet

     _-----_
    |       |    .--------------------------.
    |--(o)--|    |      Welcome to the      |
   `---------´   |   marvellous ASP.NET 5   |
    ( _´U`_ )    |        generator!        |
    /___A___\    '--------------------------'
     |  ~  |
   __'.___.'__
 ´   `  |° ´ Y `

? What type of application do you want to create? (Use arrow keys)
> Empty Application
  Console Application
  Web Application
  Web Application Basic [without Membership and Authorization]
  Web API Application
  Nancy ASP.NET Application
  Class Library
  Unit test project
{% endhighlight bat %}

At this point we want to generate a Web Application, so you can either use the arrow keys, or simply type 3 which corresponds to the third entry; then hit Enter

You will be asked to specify a name for the project, I've just chosen to call it TheWorld

{% highlight bat %}
? What's the name of your ASP.NET application? TheWorld
{% endhighlight bat %}

Have a look into the directory you generated the files into and you'll notice all the standard project files are generated for you.

{% highlight bat %}
C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\vscode\TheWorld>dir
 Volume in drive C has no label.
 Volume Serial Number is 2A20-B603

 Directory of C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\vscode\TheWorld

12/12/2015  04:17 PM    <DIR>          .
12/12/2015  04:17 PM    <DIR>          ..
12/12/2015  04:17 PM                33 .bowerrc
12/12/2015  04:17 PM             3,090 .gitignore
12/12/2015  04:17 PM               167 appsettings.json
12/12/2015  04:17 PM               198 bower.json
12/12/2015  04:17 PM    <DIR>          Controllers
12/12/2015  04:17 PM               146 Dockerfile
12/12/2015  04:17 PM             1,189 gulpfile.js
12/12/2015  04:17 PM    <DIR>          Migrations
12/12/2015  04:17 PM    <DIR>          Models
12/12/2015  04:17 PM               203 package.json
12/12/2015  04:17 PM             1,884 project.json
12/12/2015  04:17 PM             2,722 README.md
12/12/2015  04:17 PM    <DIR>          Services
12/12/2015  04:17 PM             4,116 Startup.cs
12/12/2015  04:17 PM    <DIR>          ViewModels
12/12/2015  04:17 PM    <DIR>          Views
12/12/2015  04:17 PM    <DIR>          wwwroot
              10 File(s)         13,748 bytes
               9 Dir(s)  43,917,889,536 bytes free
{% endhighlight bat %}

***

## Using Visual Studio Code (VS Code)

***

Next we want to open this project in VSCode. This can easily be done by simply typing:

{% highlight bat %}
C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\vscode\TheWorld>code .
{% endhighlight bat %}

You will be greeted with the lightweight visual studio code suite with the files we just generated loaded into the `Working File` menu.

Go ahead and open the project.json file (you'll recognize this file from the previous tutorial) and have a look at its contents.

The first block is some of the simple project specifications relating to version and namespace.

{% highlight js %}
{
  "version": "1.0.0-*",
  "userSecretsId": "aspnet5-TheWorld-8d40ceab-225c-43dd-b17b-c58e50e50d29",
  "compilationOptions": {
    "emitEntryPoint": true
  },
  "tooling": {
    "defaultNamespace": "TheWorld"
  },
{% endhighlight js %}

The next block is one we'll be using quite regularly, the dependencies for the project listing the `NuGets` we're going to use when building our application.

{% highlight js %}
"dependencies": {
  "EntityFramework.Commands": "7.0.0-rc1-final",
  "EntityFramework.SQLite": "7.0.0-rc1-final",
  "EntityFramework.MicrosoftSqlServer": "7.0.0-rc1-final",
  "Microsoft.AspNet.Authentication.Cookies": "1.0.0-rc1-final",
  "Microsoft.AspNet.Diagnostics.Entity": "7.0.0-rc1-final",
  "Microsoft.AspNet.Identity.EntityFramework": "3.0.0-rc1-final",
  "Microsoft.AspNet.IISPlatformHandler": "1.0.0-rc1-final",
  "Microsoft.AspNet.Mvc": "6.0.0-rc1-final",
  "Microsoft.AspNet.Mvc.TagHelpers": "6.0.0-rc1-final",
  "Microsoft.AspNet.Server.Kestrel": "1.0.0-rc1-final",
  "Microsoft.AspNet.StaticFiles": "1.0.0-rc1-final",
  "Microsoft.AspNet.Tooling.Razor": "1.0.0-rc1-final",
  "Microsoft.Dnx.Runtime":"1.0.0-rc1-final",
  "Microsoft.Extensions.CodeGenerators.Mvc": "1.0.0-rc1-final",
  "Microsoft.Extensions.Configuration.FileProviderExtensions" : "1.0.0-rc1-final",
  "Microsoft.Extensions.Configuration.Json": "1.0.0-rc1-final",
  "Microsoft.Extensions.Configuration.UserSecrets": "1.0.0-rc1-final",
  "Microsoft.Extensions.Logging": "1.0.0-rc1-final",
  "Microsoft.Extensions.Logging.Console": "1.0.0-rc1-final",
  "Microsoft.Extensions.Logging.Debug": "1.0.0-rc1-final"
},
{% endhighlight js %}

Finally we've got some specified commands and housekeeping code.

{% highlight js %}
"commands": {
  "web": "Microsoft.AspNet.Server.Kestrel",
  "ef": "EntityFramework.Commands"
},

"frameworks": {
  "dnx451": { },
  "dnxcore50": { }
},

"exclude": [
  "wwwroot",
  "node_modules",
  "bower_components"
],
"publishExclude": [
  "**.user",
  "**.vspscc"
],
"scripts": {
  "prepublish": [
    "npm install",
    "bower install",
    "gulp clean",
    "gulp min"
  ]
}
}
{% endhighlight js %}

Next open up the `Startup.cs` file. You'll notice along the top bar that there will be unresolved dependencies which you'll be asked to resolve by clicking Restore.

A terminal windows will spring open and begin downloading and installing all the required `NuGet` packages. If this is your first time using these `NuGet` packages, there's a good chance it'll take some time to install.

{% highlight bat %}
Restore complete, 167284ms elapsed

NuGet Config files used:
    C:\Users\Nathan\AppData\Roaming\NuGet\nuget.config

Feeds used:
    https://api.nuget.org/v3-flatcontainer/

Installed:
    265 package(s) to C:\Users\Nathan\.dnx\packages
{% endhighlight bat %}

You will now get intellisense on most of your project! Yay!.

Just back into the project.json file and have a look at the code between the 'commands' braces. You should see two:

{% highlight js %}
"web": "Microsoft.AspNet.Server.Kestrel",
"ef": "EntityFramework.Commands"
{% endhighlight js %}

So let's give one of them a go. Open your terminal windows again and run the following:

{% highlight bat %}
C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\vscode\TheWorld>dnx web
{% endhighlight bat %}

Near the end of the output you'll see the following lines that indicate that you have a webserver running on port 5000 hosted on the local machine.

{% highlight bat %}
Hosting environment: Production
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
{% endhighlight bat %}

Go ahead and open your web browser to the specified location and you'll be greeted with a wonderful splash screen that is actually your ASP.NET instance running from the command line. You can view live requests chains in the terminal windows as you navigate around the splash screen.

{% highlight bat %}
info: Microsoft.AspNet.StaticFiles.StaticFileMiddleware[2]
      Sending file. Request path: '/js/site.min.js'. Physical path: 'C:\Users\Nathan\OneDrive\02 - Code\15 - ASP.NET\vscode\TheWorld\wwwroot\js\site.min.js'
info: Microsoft.AspNet.Hosting.Internal.HostingEngine[2]
      Request finished in 0.3562ms 200 image/png
info: Microsoft.AspNet.Hosting.Internal.HostingEngine[2]
      Request finished in 0.375ms 200 image/png
info: Microsoft.AspNet.Hosting.Internal.HostingEngine[2]
      Request finished in 0.1687ms 200 image/png
info: Microsoft.AspNet.Hosting.Internal.HostingEngine[2]
      Request finished in 0.1484ms 200 application/javascript
info: Microsoft.AspNet.Hosting.Internal.HostingEngine[2]
      Request finished in 0.2016ms 200 image/png
{% endhighlight bat %}

***

## Summary

***

We've learned how to generate code using Yeoman and edit/view it in `Visual Studio Code`. In the next lesson we'll be learning how to do the same thing, from scratch in `Visual Studio 2015`. Hopefully I'm not going into too much detail, but I think it's beneficial to have the whole process documented instead of assuming everything will understand exactly what's going on.

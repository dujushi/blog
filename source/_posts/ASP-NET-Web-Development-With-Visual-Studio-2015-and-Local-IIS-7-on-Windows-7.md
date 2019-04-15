layout: blog
title: ASP.NET Web Development With Visual Studio 2015 and Local IIS 7 on Windows 7
date: 2015-11-12 21:41:30
tags: 
- Visual Studio 2015
- Local IIS
- Windows 7
categories: 
- ASP.NET MVC
---
Visual Studio 2015 provides ASP.NET web development environment directly with IIS Express. It is very straight forward to start web development with this setting. But if you ever used Visual Studio 2015 with Local IIS, you would find that it is much more convenient and you would love it.

This tutorial will go through how to set up ASP.NET web development environment with Visual Studio 2015 and Local IIS on Windows 7 briefly.<!-- more -->

### Install IIS 7 on Windows 7
Windows 7 comes with IIS support. But you need to turn on the features from Control Panel. Follow this [link](http://www.iis.net/learn/install/installing-iis-7/installing-iis-on-windows-vista-and-windows-7) to set it up.

You can also install IIS extensions and popular applications (such as [Umbraco](http://umbraco.com), the famous CMS application with .NET) with Microsoft Web Platform Installer. Check this [link](https://www.microsoft.com/web/downloads/platform.aspx) out.

### Create a new website with Visual Studio 2015
You can simply create one with the default MVC template.

### Create a new host on IIS
Open IIS Manager. And then follow this [link](https://www.microsoft.com/web/downloads/platform.aspx) to set up a new site.

### Configure VS to use Local IIS
In VS, open Properties under Solution Explorer. Go to Web tab. Update Servers to use Local IIS. Enter the Project URL you set up in step 3. Add the host binding in hosts file. Now you are ready to run the new site.

### Trouble Shootings
If you get blank page, make sure your application pool is using the latest .NET framework. Go to Application Pools in IIS Manager and fix the setting. Or go to your site setting in IIS, check your Handler Mappings have .cshtml. Otherwise, you may need to reinstall .NET framework with this command:

> aspnet_regiis -i

Check this [link](https://www.microsoft.com/web/downloads/platform.aspx) for more info.

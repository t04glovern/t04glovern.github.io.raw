---
layout: post
title: "EqualLogic storage pool connection via iSCSI into ESXi"
date: "2016-02-02"
comments: true
description: The process required to connect EqualLogic storage pools into ESXi via iSCSI
share: true
author: nathan
tags:
 - equallogic
 - esxi
 - iscsi
---

## Introduction

***

This guide outlines how to get `Borrowed Space` storage pools created in `EqualLogic storage manager` talking with ESXi over an iSCSI link. I make the assumption that you already have the various storage pools setup, my guide uses three existing spaces called Gold, Silver and Bronze.

***

## Setup an iSCSI host adaptor

***

Login to one of the ESXi boxes you want to configure with the vSphere client and navigate to the `Configuration tab > Storage Adapters`. You'll be greeted with various storage adapters that are currently attached to your physical system. In order for us to talk with the EqualLogic storage device we need an iSCSI adapter setup.

![Adapter Creation]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-adapter.png)

Right click and select `Add Software iSCSI Adapter` then hit OK on the screen alerting you to the creation of the new adapter. Awesome, now we have an adapter that will handle our storage traffic between the EqualLogic and our hosts.

Go ahead and check out the properties of the new adapter. Take note (copy) the iSCSI Name. It'll be a very long string that uniquely defines the interface; we'll be using it shortly so make sure you've copied it down somewhere.

![Adapter Creation with Name]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-adapter-name.png)

***

## EqualLogic Access policies

***

Login to the `EqualLogic Group Manager` and jump into the `Group Configuration` section. You can see I've already setup the pools to connect with my first ESXi host.

![Group Manager Access Policies]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-access-policies.png)

Next open up the Access Policies section and check `New` under `Access Policies`. Give it a common name that make sense, generally the name of the ESXi host that you're connecting to makes sense.

![Group Manager Access Policies Name]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-access-policies-name.png)

Click `New` to add a new access point. The only box we'll worry about here is the `iSCSI Initiator Name`, which will be the name of the adaptor I told you to copy down before. Punch that in and click `Ok` and then `Ok` on the remaining windows.

![Group Manager Access Policies iSCSI Name]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-access-policies-iscsi-name.png)

Now all we need to do is assign various Space targets to the policies. Do this by selecting the new Policy we just created and clicking `Add` in the `Targets` menu (top right)

![Group Manager Access Policies Target]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-access-policies-targets.png)

Simply check all the various volumes you'd like to be available to the host connected to the policy we just setup and click `Ok`

![Group Manager Access Policies Target Select]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-access-policies-targets-select.png)

***

## Connect iSCSI Storage

***

Back in ESXi right click and select `Properites` on your `iSCSI Software Adapter` and click the `Dynamic Discovery` tab. Add a new location and put in the IP Address of the EqualLogic group manager (port is standard iSCSI 3260) and click `Ok`

![ESXi iSCSI Dynamic Discovery]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-adapter-discovery.png)

Click on Static Discovery and you'll likely already see the three storage targets we setup prior

![ESXi iSCSI Static Discovery]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-adapter-discovery-static.png)

Now we just need to add the discovered devices as storage locations; do this by navigating to the `Configuration > Storage` section. You might find that the Datastores already show up and have been added automatically. If they have, then don't worry about the following steps (you're done!)

If they don't show up simply click `Add Storage` with the storage type `Disk/LUN` and individually add the three targets making sure to name them something reasonable.

![ESXi iSCSI Datastores]({{ site.url }}/images/posts/2016.02.02/equallogic-storage-pool-added.png)

***

## Summary

***

Using `iSCSI Datastores` mean you can leverage some great `vMotion` and `HA` features that exist in vSphere without having to worry about large data migrations during VM moves. It's also a great way to ensure that all your various storage arrays are somewhat centralized.

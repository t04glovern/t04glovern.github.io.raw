---
layout: post
title: "Python - Virtualenv Quickstart"
date: "2016-02-03"
comments: true
description: An extremely quickstart guide to virtualenv
share: true
author: nathan
tags:
 - python
 - virtualenv
---

## Introduction

***

Virtualenv is a tool that is used to isolated Python environments. It does this by creating a folder that contains all the necessary executables to use the packages that a Python project would need.

***

## Installation

***

You can install virtualenv via pip

{% highlight bash %}
$ pip install virtualenv
{% endhighlight bash %}

***

## Basic Usage

***

Create a virtual environment for a project

{% highlight bash %}
$ cd project_folder
$ virtualenv venv
{% endhighlight bash %}

This will create a folder in the current working directory that will contain a copy of all your python executable files from the current default python on your system

You can specify which python interpreter you'd like to use as well:

{% highlight bash %}
$ virtualenv -p /usr/bin/python2.7 venv
{% endhighlight bash %}

To start up your environment, activate it with the following

{% highlight bash %}
$ source venv/bin/activate
{% endhighlight bash %}

The name of the current virtual environment will now appear on the left of the prompt to let you know it's active

![Virtualenv]({{ site.url }}/images/posts/2016.02.03/python-virtualenv-path.png)

***

## Removing environment

***

In order to remove and envinroment, simple `rm -rf` the virtualenv folder in your project; or delete the project folder all together.

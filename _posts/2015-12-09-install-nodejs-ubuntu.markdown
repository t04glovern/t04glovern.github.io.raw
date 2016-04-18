---
layout: post
title: "Installing Node.js the easy way"
date: "2015-12-09"
comments: true
description: A simple method for installing any version of Node.js using bash.
share: true
author: nathan
tags:
 - node.js
 - bash
 - script
---

## Problem

***

It's not uncommon to see `Node.js` listed as a requirement for packages; In the past I would simply attempt an install with a package manager and cross my fingers that the build given to be was correct, while also worrying about the location and environment variables not being setup correctly.

Recently I got fed up while trying to build a particular package I was interested in and decided to track down a more permanent solution to this reoccurring problem.

***

## Solution

***

After some google-fu (or more specifically, DuckDuck-fu?) I came across the following script.

{% highlight bash %}
#! /bin/bash
# run it by: bash install-node.sh
read -p " which version of Node do you need to install: enter 0.10.24, 0.10.28, or 4.2.2 (or any other valid version): " VERSIONNAME
read -p " Are you using a 32-bit or 64-bit operating system ? Enter 64 or 32: " ARCHVALUE
if [[ $ARCHVALUE = 32 ]]
    then
    printf "user put in 32 \n"
    ARCHVALUE=86
    URL=http://nodejs.org/dist/v${VERSIONNAME}/node-v${VERSIONNAME}-linux-x${ARCHVALUE}.tar.gz

elif [[ $ARCHVALUE = 64 ]]
    then
    printf "user put in 64 \n"
    ARCHVALUE=64
    URL=http://nodejs.org/dist/v${VERSIONNAME}/node-v${VERSIONNAME}-linux-x${ARCHVALUE}.tar.gz

else
    printf "invalid input expted either 32 or 64 as input, quitting ... \n"

    exit
fi

# setting up the folders and the the symbolic links
printf $URL"\n"
ME=$(whoami) ; sudo chown -R $ME /usr/local && cd /usr/local/bin #adding yourself to the group to access /usr/local/bin
mkdir _node && cd $_ && wget $URL -O - | tar zxf - --strip-components=1 # downloads and unzips the content to _node
cp -r ./lib/node_modules/ /usr/local/lib/ # copy the node modules folder to the /lib/ folder
cp -r ./include/node /usr/local/include/ # copy the /include/node folder to /usr/local/include folder
mkdir /usr/local/man/man1 # create the man folder
cp ./share/man/man1/node.1 /usr/local/man/man1/ # copy the man file
cp bin/node /usr/local/bin/ # copy node to the bin folder
ln -s "/usr/local/lib/node_modules/npm/bin/npm-cli.js" ../npm ## making the symbolic link to npm
# print the version of node and npm
node -v
npm -v
{% endhighlight bash %}

When you run the script you'll be prompted asking for the version (numbers can be found here: https://nodejs.org/en/download/) and the architecture you're working on. Simply put in your desired values and presto! you'll have the correct version installed and ready to go.

You can check the version your running using the following command:

{% highlight bash %}
node --version
{% endhighlight bash %}

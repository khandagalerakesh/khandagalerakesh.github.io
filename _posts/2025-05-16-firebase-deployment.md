---
layout: post
title:  "Firebase Deployment - Part 1"
date:   2025-05-16 10:00:00 +0000
categories: blog
---

### **Pre-requisite**

- Make sure that you have setup your app on Firebase and downloaded GoogleService-Info.plist
- Make sure that you have Ruby installed
- Make sure that you have Xcode installed
- Make sure that you have Xcode Command Line Tools(CLT) installed
- Make sure you have logged into Xcode accounts using the dev account, and synced certs & provisioning profiles
- Install fastlane on your system using the below command in terminal. Enter system password when prompted

`sudo gem install -n /usr/local/bin fastlane —-verbose`

`fastlane init`
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

# **FastLane SetUp**

- In terminal, go to your project folder & execute below command to setup fastlane for your project

`fastlane init`

- When prompted, manually setup your project to automate your tasks. Enter the digit against it, we will set up manually. **In this case above enter 4**.

- In the project folder, you’ll see a few new things:
1. **Gemfile**, which includes the fastlane gem as a project dependency
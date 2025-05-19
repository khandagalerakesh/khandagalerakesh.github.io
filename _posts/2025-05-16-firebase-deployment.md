---
layout: post
title:  "Firebase Deployment - Part 1"
date:   2025-05-16 10:00:00 +0000
categories: blog
---

# **Pre-requisite**

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

- A fastlane folder containing:
1. **Appfile**: stores the app identifier, your Apple ID and any other identifying information that fastlane needs to set up your app
2. **Fastfile**: manages the lanes you’ll create to invoke fastlane actions

# **Plugin File**

- In the plugin file, add the firebase distribution plugin. Paste the below line in the plugin file to add support for firebase distribution

`gem 'fastlane-plugin-firebase_app_distribution'`

# **Creating the IPA file**

- In terminal, go to your project folder & execute below command to generate a Gymfile 

`fastlane gym init`

- Open Gymfile in your text editor, disable smart quotes, and replace the contents of the file with code associated to your project needs and requirements


```
output_directory("<<PATH_FOR_SAVING_IPA_AND_DSYM_FILES>>") 
export_method("<<EXPORT_METHOD>>")
include_bitcode(<<BOOL>>)
include_symbols(<<BOOL>>)
export_xcargs("-allowProvisioningUpdates")
```
1. Specifies where fastlane should store the .ipa app binary file
2. Builds an Ad-Hoc build for QA testing 
3. Includes bitcode from the build. Bitcode allows Apple to optimize your app.
4. Includes symbols from the build. Including symbols allows Apple to access the app’s debug information.
5. Allows Xcode to use automatic provisioning.

- Open Fastfile in a text editor, disable smart quotes, and replace the contents of the file with code associated to your project needs and requirements


```
default_platform(:ios)

platform :ios do
     desc "Create ipa"
     lane :build_qa_ipa do
       build_app(
         scheme: "<<SCHEME_NAME>>",
         configuration: "<<BUILD_CONFIGURATION>>",
         export_method: "<<EXPORT_METHOD>>",
         output_name: "<<OUTPUT_NAME>>",
         export_options: "<<EXPORT_OPTIONS>>"
       )
    end
end
```

1.  Provides description for the lane. Lane is collection of commands which run in sequence.
2.  Lane name is “build_qa_ipa”.
3.  Runs the build_app command with the provided parameters

- Open terminal, go to your project directory, run:

`fastlane build_qa_ipa`

- This will create build folder containing .ipa file and .dSYM zip file.

# **Upload to firebase**

- In terminal, go to your project folder & execute below command to add the app distribution plugin for fastlane installation

`fastlane add_plugin firebase_app_distribution` 

- Open Fastfile in a text editor, add below code for distributing the build firebase


```
desc "Upload dev to Firebase"
lane :upload_firebase do
     firebase_app_distribution(
        app: "<<APP_ID>>",
        googleservice_info_plist_path: "<<GOOGLE_SERVICE_INFO_PLIST_FILE>>",
        service_credentials_file: "<<SERVICE_CREDENTIALS_JSON_FILE>>",
        groups_file: "<<TESTER_GROUPS_File>>",
        ipa_path: "<<IPA_FILE_PATH>>"
     )
end
```

1. Firebase upload lane name
2. Invokes the firebase_app_distribution action.
3. Firebase app id
4. Google Service Info plist file path within the project directory
5. Service Credentials File which is a json file saved withing the project directory
6. Groups File which contains tester groups in a comma separated value
7. IPA file path

- After all the above setup, run the following command from Terminal:

`fastlane upload_firebase`

- This will upload the ipa file to Firebase project and send out an email to all the members in the tester groups mentioned in the groups file


# **Fastlane Environments**

- Fastlane environments provide a way to customize our fastlane lanes to work with many different configurations and allow for easily swapping between them all.
- In our Fastfile we can access the environment using the ENV variable, which is a dictionary available to all of our lanes.
- Create an .env file for a specific build configuration. For example, for a Debug/QA build configuration create an environment file as `.env.Debug`


```
BUILD_SCHEME = "<<SCHEME_NAME>>"
BUILD_CONFIGURATION = "<<BUILD_CONFIG>>"
EXPORT_METHOD = "<<EXPORT_METHOD>>"
OUTPUT_NAME = "<<IPA_FILE_NAME>>"
EXPORT_OPTIONS = "<<EXPORT_OPTIONS_FILE_WITH_PROFILE_DETAILS>>"
APP_IDENTIFIER = "<<BUNDLE_IDENTIFIER>>"
```

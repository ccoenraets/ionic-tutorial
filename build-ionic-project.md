---
layout: module
title: Module 8&#58; Building the Application
---

This module is optional. To build the application for iOS and/or Android, you need the iOS and/or Android SDK 
installed on your system.

## Building for iOS

> You need the iOS SDK installed on your computer to build an iOS version of your application 
using the steps below.

1. On the command line, navigate to the **ionic-tutorial/conference** directory

1. Add support for the iOS platform:

    ```
    ionic platform add ios
    ```

1. Build the project:

    ```
    ionic build ios
    ```

1. Open **Workshop.xcodeproj** in the **workshop/platforms/ios** directory

1. In Xcode, run the application on a device connected to your computer or in the iOS emulator


## Building for Android

> You need the Android SDK installed on your computer to build an Android version of your 
application using the steps below.

1. On the command line, navigate to the **ionic-tutorial/conference** directory

1. Add support for the Android platform:

    ```
    ionic platform add android
    ```

1. Build the project:

    ```
    ionic build android
    ```

    The project is built in the **conference/platforms/android** folder


1. To build and run the application on an Android device connected to your computer using a USB cable:

    ```
    ionic run android
    ```

1. To build and run the application in the Android emulator:

    ```
    ionic emulate android
    ```


<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="angular-ui-router.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 
Previous</a>
<a href="ionic-facebook-integration.html" class="btn btn-default pull-right">Next <i class="glyphicon 
glyphicon-chevron-right"></i></a>
</div>
</div>



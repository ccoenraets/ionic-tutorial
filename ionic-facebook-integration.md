---
layout: module
title: Module 9&#58; Facebook Integration
---
In this module, you add Facebook integration to your application. You allow users to login with Facebook, 
view their profile, and share their favorite sessions on their feed.

> In this tutorial, you use [OpenFB](https://github.com/ccoenraets/OpenFB) to perform the integration. OpenFB is a 
micro-library that lets you integrate 
your JavaScript applications with Facebook. It works for both browser-based and Cordova/PhoneGap apps. It also 
doesn't have any dependency: You don't need the Facebook plugin when running in Cordova. You also don't need the 
Facebook SDK. More information [here](https://github.com/ccoenraets/OpenFB).

## Step 1: Create a Facebook application

1. Login to Facebook

1. Access [https://developers.facebook.com/apps](https://developers.facebook.com/apps), and click **Add New App**

1. Select **www** as the platform

1. Type a unique name for your app and click **Create New Facebook App ID** 

1. Specify a **Category**, and click **Create App ID**

1. Click **My Apps** in the menu and select the app you just created 

1. Click **Settings** in the left navigation

1. Click the **Advanced Tab**

1. In the **OAuth Settings** section, add the following URLs in the **Valid OAuth redirect URIs** field:
    - [http://localhost:8100/oauthcallback.html](http://localhost:8100/oauthcallback.html) (for access using ionic serve)
    - [https://www.facebook.com/connect/login_success.html](https://www.facebook.com/connect/login_success.html) (for access from Cordova)

1. Click **Save Changes**  


## Step 2: Initialize OpenFB

1. Add the OpenFB files to your application
    - Copy **openfb.js** and **ngopenfb.js** from **ionic-tutorial/resources** to **conference/www/js**.
    - Copy **oauthcallback.html** and **logoutcallback.html** from **ionic-tutorial/resources** to **conference/www**.
    - In **conference/www/index.html**, add script tags to include openfb.js and ngopenfb.js (before app.js):

        ```
        <script src="js/openfb.js"></script>
        <script src="js/ngopenfb.js"></script>
        ```

        > ngOpenFB is just an Angular wrapper around the OpenFB library. It lets you use OpenFB "the Angular way":
            as an Angular service (called ngFB) instead of a global object, and using promises instead of callbacks.

1. Open **conference/www/js/app.js**, and add **ngOpenFB** as a dependency to the **starter** module:
 
    ```
    angular.module('starter', ['ionic', 'starter.controllers', 'ngOpenFB'])
    ```

1. Inject **ngFB** in the run() function declaration:

    ```
    .run(function ($ionicPlatform, ngFB) {
    ```
 
 
1. Initialize OpenFB on the first line of the run() function. Replace **YOUR&#95;FB&#95;APP_ID** with the App ID of your Facebook application.

    ```
    ngFB.init({appId: 'YOUR_FB_APP_ID'});
    ```

## Step 3: Add Facebook login

1. Open login.html in the **www/conference/templates** directory. Add a **Login with Facebook** button right after the 
existing **Log 
In** button: 

    ```
    <label class="item">
        <button class="button button-block button-positive" ng-click="fbLogin()">
        Login with Facebook
        </button>
    </label>
    ```

    > Notice that fbLogin() is called on ng-click. You define the fbLogin() function in the next step.

1. Open conference/www/js/controllers.js, and add **ngOpenFB** as a dependency to the **starter.controllers** module:
 
    ```
    angular.module('starter.controllers', ['starter.services', 'ngOpenFB'])
    ``` 

1. Inject **ngFB** in the **AppCtrl** controller:

    ```
    .controller('AppCtrl', function ($scope, $ionicModal, $timeout, ngFB) {
    ```
    
1. Add the fbLogin function in the **AppCtrl** controller (right after the 
doLogin function):

    ```
    $scope.fbLogin = function () {
        ngFB.login({scope: 'email,read_stream,publish_actions'}).then(
            function (response) {
                if (response.status === 'connected') {
                    console.log('Facebook login succeeded');
                    $scope.closeLogin();
                } else {
                    alert('Facebook login failed');
                }
            });
    };
    ```

1. Test the application. 
    - Make sure **ionic serve** (your local web server) is still running. If it's running but you closed your app page in the browser, you can reload the app by accessing the following URL: [http://localhost:8100](http://localhost:8100). 
        If it's not running, open a command prompt, navigate (cd) to the **ionic-tutorial** directory and type:
    
        ```
        ionic serve
        ```
    - In the application, open the side menu and select **Login**
    - Click the **Login with Facebook** button
    - Enter your Facebook credentials on the Facebook login screen, and authorize the application 
    - Open the browser console: you should see the **Facebook login succeeded** message

    > The next time you login, you won't be asked for your credentials again since you already have a valid token. To
     test the login process again, simply logout from Facebook. The OpenFB library has additional methods to logout 
     and revoke permissions that are beyond the scope of this tutorial.  


## Step 4: Display the User Profile

1. Create a **template** for the user profile view. In the **conference/www/templates** directory, create a new file named **profile.html** and implement it as follows:

    ```
    {% raw %}
    <ion-view view-title="Profile">
        <ion-content class="has-header">
            <div class="list card">
                <div class="item">
                    <h2>{{user.name}}</h2>
                    <p>{{user.city}}</p>
                </div>
                <div class="item item-body">
                    <img src="http://graph.facebook.com/{{user.id}}/picture?width=270&height=270"/>
                </div>
            </div>
        </ion-content>
    </ion-view>
    {% endraw %}
    ```

1. Create a **controller** for the user profile view. Open **controllers.js**, and add the following controller:

    ```
    .controller('ProfileCtrl', function ($scope, ngFB) {
        ngFB.api({
            path: '/me',
            params: {fields: 'id,name'}
        }).then(
            function (user) {
                $scope.user = user;
            },
            function (error) {
                alert('Facebook error: ' + error.error_description);
            });
    });
    ```

1. Create a **route** for the user profile view. Open **app.js**, and add the following route:

    ```
    .state('app.profile', {
        url: "/profile",
        views: {
            'menuContent': {
                templateUrl: "templates/profile.html",
                controller: "ProfileCtrl"
            }
        }
    })
    ```

1. Open **www/templates/menu.html**, and add the following menu item:

    ```
    <ion-item menu-close href="#/app/profile">
        Profile
    </ion-item>
    ```

1. Test the application:
    - Make sure **ionic serve** (your local web server) is still running. If it's running but you closed your app page in the browser, you can reload the app by accessing the following URL: [http://localhost:8100](http://localhost:8100). 
        If it's not running, open a command prompt, navigate (cd) to the **ionic-tutorial** directory and type:
    
        ```
        ionic serve
        ```
    - Open the side menu and select **Login**
    - Login with Facebook
    - Open the side menu and select **Profile**


## Step 5: Publish to your feed

1. Open **controllers.js**, and inject **ngFB** in the **SessionCtrl** definition:
 
    ```
    .controller('SessionCtrl', function ($scope, $stateParams, Session, ngFB) {
    ```

1. Add a **share()** function to the **SessionCtrl** controller:

    ```
    $scope.share = function (event) {
        ngFB.api({
            method: 'POST',
            path: '/me/feed',
            params: {
                message: "I'll be attending: '" + $scope.session.title + "' by " +
                $scope.session.speaker
            }
        }).then(
            function () {
                alert('The session was shared on Facebook');
            },
            function () {
                alert('An error occurred while sharing this session on Facebook');
            });
    };
    ```

1. Open **session.html** in the templates directory and add an **ng-click** handler to the Share button: invoke the 
share function: 

    ```
    <a class="tab-item" ng-click="share()">
        <i class="icon ion-share"></i>
        Share
    </a>
    ```

1. Test the application:
    - Make sure **ionic serve** (your local web server) is still running. If it's running but you closed your app page in the browser, you can reload the app by accessing the following URL: [http://localhost:8100](http://localhost:8100). 
        If it's not running, open a command prompt, navigate (cd) to the **ionic-tutorial** directory and type:
    
        ```
        ionic serve
        ```
    - Open the side menu and select **Login**
    - Login with Facebook
    - Open the side menu, select **Sessions**, and select a session in the list
    - Click/Tap the **Share** button
    - Check your feed on Facebook


## Step 6: Test Facebook integration on device (optional)

1. Add the **InAppBrowser** plugin used by OpenFB when running in Cordova. On the command line, navigate to the **ionic-tutorial/conference** directory and type:

    ```
    cordova plugins add org.apache.cordova.inappbrowser
    ```

1. Build your application for a specific platform following the steps described in module 8:

    ```
    ionic build ios
    ```

    and/or
 
    ```
    ionic build android
    ```
    
    If the build fails after adding the plugin, try removing and re-adding the platform. For example:
    
    ```
    ionic platform remove ios
    ionic platform add ios
    ionic build ios
    ```
 
1. Run and test your application on an iOS or Android device or emulator 

<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="build-ionic-project.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 
Previous</a>
</div>
</div>
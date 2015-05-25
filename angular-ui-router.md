---
layout: module
title: Module 7&#58; Implementing Routing
---
In this module, you add two new routes (states) to the application: app.sessions loads the session list 
view, and app.session loads the session details view.

## Step 1: Define the app.sessions route 

1. Open **app.js** in conference/www/js

1. Delete the **app.playlists** state
 
1. Replace it with an **app.sessions** state defined as follows:

    ```
    .state('app.sessions', {
      url: "/sessions",
      views: {
          'menuContent': {
              templateUrl: "templates/sessions.html",
              controller: 'SessionsCtrl'
          }
      }
    })
    ```

## Step 2: Define the app.session route 

1. Delete the **app.single** state
 
1. Replace it with an app.session state defined as follows:

    ```
    .state('app.session', {
        url: "/sessions/:sessionId",
        views: {
            'menuContent': {
              templateUrl: "templates/session.html",
              controller: 'SessionCtrl'
          }
        }
    });
    ```

## Step 3: Modify the default route 

Modify the fallback route to default to the list of sessions (last line in app.js):

```
$urlRouterProvider.otherwise('/app/sessions');
```

## Step 4: Modify the side menu 

1. Open **menu.html** in conference/www/templates

1. Modify the Playlists menu item as follows (modify both the item label and the **href**):

    ```
    <ion-item href="#/app/sessions">
        Sessions
    </ion-item>
    ```

## Step 5: Test the application

1. Make sure **ionic serve** (your local web server) is still running.
    - If it's running but you closed your app page in the browser, you can reload the app by loading the following URL: [http://localhost:8100](http://localhost:8100)
    - If it's not running, open a command prompt, navigate (cd) to the **ionic-tutorial** directory and type:

        ```
        ionic serve
        ```
    
1. In the conference application, open the side menu ("Hamburger" icon in the upper left corner) and select **Sessions**. Select a session in the list
 to see the session details.


<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="create-ionic-template.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 
Previous</a>
<a href="build-ionic-project.html" class="btn btn-default pull-right">Next <i class="glyphicon 
glyphicon-chevron-right"></i></a>
</div>
</div>



# Ionic Tutorial

In this tutorial, you build a conference application that allows conference attendees to browse through the list of sessions. The Node.js backend provided with the supporting files exposes the conference data through a set of REST services.

## Step 1: Install Ionic

1. Make sure you have an up-to-date version of Node.js installed on your system. If you don't have Node.js installed on your system, you can install it from [here](http://nodejs.org/).

1. Open a terminal window (Mac) or a Command window (Windows), and install Cordova and Ionic:

  ```
  npm install -g cordova ionic
  ```

  On a Mac, you may have to use **sudo** depending on your system configuration:

  ```
  sudo npm install -g cordova ionic
  ```

1. If you already have Cordova and Ionic installed on your computer, make sure you update to the latest version:

  ```
  npm update -g cordova ionic
  ```

  or

  ```
  sudo npm update -g cordova ionic
  ```


## Step 2: Install and Start the Conference Server

1. Download the supporting files for this tutorial [here](https://github.com/ccoenraets/ionic-tutorial/archive/master.zip), or clone the repository:

    ```
    git clone https://github.com/ccoenraets/ionic-tutorial
    ```

  If you downloaded the zip file, unzip it anywhere on your file system.

  > The supporting files feature a Node.js backend that exposes the conference data through a set of REST services.

1. Open a terminal window (Mac) or a Command window (Windows), and navigate (cd) to the **ionic-tutorial/server** directory

1. Install the server dependencies:

  ```
  npm install
  ```

1. Start the server

  ```
  node server
  ```

1. Test the REST services. Open a browser and access the following URLs:
  - http://localhost:5000/sessions (for a list of conference sessions returned as a JSON document)
  - http://localhost:5000/sessions/1 (for information about a specific session )

## Step 3: Create the Application

1. Open another terminal window (Mac) or a Command window (Windows), and navigate (cd) to the **ionic-tutorial** directory

1. Using the Ionic CLI, create an application named **conference** based on the **sidemenu** starter app:

  ```
  ionic start conference sidemenu
  ```

1. Try the starter application. Open a browser and access the following URL:

  http://localhost:5000

  This works because the Node.js server is configured to serve static pages in the conference/www folder. Open server.js in ionic-tutorial/server to see the code. Specifically:

  ```
  app.use(express.static('../conference/www'));
  ```

1. In the application, open the side menu and select **Playlists**. Select a playlist in the list to see the details view.

  > In this tutorial, you will replace the playlists with a list of conference sessions retrieved from the server using the REST services you experimented with in the previous step.


## Step 4: Create the Session Service

In the sidemenu starter app, the playlists are hardcoded in controllers.js. In this step, you create a Session service that uses the [Angular Resource module](https://docs.angularjs.org/api/ngResource/service/$resource) (ngResource) to retrieve the conference sessions using REST services.

1. In the **js** folder, create a file named **services.js**

2. In services.js, define a **module** named **starter.services** with a dependency on ngResource. In that module, define a **service** named **Session** that provides access to the REST services at the specified endpoint:

  ```
  angular.module('starter.services', ['ngResource'])

  .factory('Session', function ($resource) {
    return $resource('http://localhost:5000/sessions/:sessionId');
  });
  ```

1. The starter.services module you just created has a dependency on the Angular Resource module (ngResource) which is not included by default. Open index.html and add a script tag to include angular-resource.min.js (right after ionic-bundle.js):

  ```
  <script src="lib/ionic/js/angular/angular-resource.min.js"></script>
  ```

1. Add a script tag to include services.js (right after app.js):

  ```
  <script src="js/services.js"></script>
  ```

## Step 5: Create the Session Controllers

[AngularJS controllers](https://docs.angularjs.org/guide/controller) act as the glue between views and services. A controller often invokes a method in a service to obtain data that it stores in the [Angular scope](https://docs.angularjs.org/guide/scope) so that it can be displayed by the view.

1. Open js/controllers.js, and add **starter.services** as a dependency to meke the Session service available to the controllers:

  ```
  angular.module('starter.controllers', ['starter.services'])
  ```

1. Delete PlayListsCtrl (plural) and replace it with a controller named SessionsCtrl that retrieves the list of conference sessions using the Session service and stores it in a scope variable named sessions:

  ```
  .controller('SessionsCtrl', function($scope, Session) {
      $scope.sessions = Session.query();
  })
  ```

1. Delete PlayListCtrl (singular) and replace it with a controller named SessionCtrl that retrieves a specific session using the Session service and stores it in a scope variable named session:

  ```
  .controller('SessionCtrl', function($scope, $stateParams, Session) {
      $scope.session = Session.get({sessionId: $stateParams.sessionId});
  });

  ```

## Step 6: Create the Templates

1. In the templates folder, rename playlists.html (plural) to sessions.html, and implement the template as follows:

  ```
  <ion-view title="Sessions">
      <ion-nav-buttons side="left">
          <button menu-toggle="left" class="button button-icon icon ion-navicon"></button>
      </ion-nav-buttons>
      <ion-content class="has-header">
          <ion-list>
              <ion-item ng-repeat="session in sessions" href="#/app/sessions/{{session.id}}">
                  {{session.title}}
              </ion-item>
          </ion-list>
      </ion-content>
  </ion-view>
  ```

  > Notice the use of the ng-repeat directive to display the list of sessions

1. Rename playlist.html (singular) to session.html and implement it as follows:

  ```
  <ion-view title="Session">
      <ion-content class="has-header">
          <div class="list card">
              <div class="item">
                  <h3>{{session.time}}</h3>
                  <h2>{{session.title}}</h2>
                  <p>{{session.speaker}}</p>
              </div>

              <div class="item item-body">
                  <p>{{session.description}}</p>
              </div>
              <div class="item tabs tabs-secondary tabs-icon-left">
                  <a class="tab-item" href="#">
                      <i class="icon ion-thumbsup"></i>
                      Like
                  </a>
                  <a class="tab-item" href="#">
                      <i class="icon ion-chatbox"></i>
                      Comment
                  </a>
                  <a class="tab-item" href="#">
                      <i class="icon ion-share"></i>
                      Share
                  </a>
              </div>
          </div>
      </ion-content>
  </ion-view>
  ```

## Step 7: Implement Routing

1. Open **app.js** in conference/www/js

1. Delete the **app.playlists** state and replace it with an **app.sessions** state defined as follows:

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

1. Delete the **app.single** state and replace it with an app.session state defined as follows:

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

1. Modify the fallback route to default to the list of sessions:

  ```
  $urlRouterProvider.otherwise('/app/sessions');
  ```

## Step 8: Modify the Side Menu

1. Open **menu.html** in conference/www/templates

1. Modify the Playlists menu item as follows (modify both the item label and href):

  ```
  <ion-item nav-clear menu-close href="#/app/sessions">
    Sessions
  </ion-item>
  ```

## Step 9: Test the Application

1. Open a browser and access the following URL:

  http://localhost:5000

1. Open the side menu and select Sessions. Select a session in the list to see the session details.

## Implement Facebook Login

1. Login to Facebook and create a Facebook application. Specify localhost:5000/oauthcallback.html as a valid OAuth Redirect URL.

1. Copy openfb.js from phonegap-workshop-jquk2014/resources to conference/www/lib.

1. Copy oauthcallback.html from phonegap-workshop-jquk2014/resources to conference/www.

4. In index.html, add a script tag to include openfb.js (before app.js):

  ```
  <script src="lib/openfb.js"></script>
  ```

1. Open www/templates/menu.html, delete the Search and Browse menu items, and add
the following menu item:

  ```
  <ion-item nav-clear menu-close href="#/app/login">
      Login
  </ion-item>
  ```

2. In the templates folder, create a new file name login.html and implement it as follows:

  ```
  <ion-view title="Login">
      <ion-nav-buttons side="left">
          <button menu-toggle="left" class="button button-icon icon ion-navicon"></button>
      </ion-nav-buttons>
      <ion-content class="has-header padding">
          <button class="button button-block button-positive" ng-click="login()">Login with Facebook</button>
      </ion-content>
  </ion-view>
  ```

3. Open app.js, and initialize OpenFB in the config() function (first line):

  ```
  openFB.init('YOUR_FB_APP_ID'); // Defaults to sessionStorage for storing the Facebook token

  ```

4. Delete the search and browse routes, and add the following route:

  ```
  .state('app.login', {
      url: "/login",
      views: {
          'menuContent' :{
              templateUrl: "templates/login.html",
              controller: "LoginCtrl"
          }
      }
  })
  ```

4. Open js/controllers.js, and add the following controller:

  ```
  .controller('LoginCtrl', function($scope) {
      $scope.login = function() {
          openFB.login('email',
              function() {
                  alert('Facebook login succeeded');
              },
              function(error) {
                  alert('Facebook login failed: ' + error.error_description);
              });
      };
  })
  ```

5. Test the application



## Display the User Profile

1. Open www/templates/menu.html, and add
the following menu item:

  ```
  <ion-item nav-clear menu-close href="#/app/profile">
      Profile
  </ion-item>
  ```

2. In the templates folder, create a new file named profile.html and implement it as follows:

  ```
  <ion-view title="Profile">
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
  ```

3. Open app.js, and add the following route:

  ```
  .state('app.profile', {
      url: "/profile",
      views: {
          'menuContent' :{
              templateUrl: "templates/profile.html",
              controller: "ProfileCtrl"
          }
      }
  })
  ```

4. Open controllers.js, and add the following controller:

  ```
  .controller('ProfileCtrl', function($scope, $q) {
      openFB.api({
          path: '/me',
          params: {fields: 'id,name'},
          success: function(user) {
              console.log(user);
              $scope.$apply(function() {
                  $scope.user = user;
              });
          },
          error: function(error) {
              console.log(error);
              alert('Facebook error: ' + error.error_description);
          }
      });
  });
  ```

5. Test the application


## Build the Project

1. Add a platform. For example:

  ```
  ionic platform add ios
  ```  

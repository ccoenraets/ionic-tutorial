---
layout: module
title: Module 4&#58; Creating the Session Service
---
In the sidemenu starter app, the playlists are hardcoded in controllers.js. In this module, 
you create a Session service that uses the [Angular resource module](https://docs.angularjs
.org/api/ngResource/service/$resource) (ngResource) to retrieve the conference sessions using REST services. 

## Steps

1. In the **conference/www/js** directory, create a file named **services.js**

2. In services.js, define a **module** named **starter.services** with a dependency on ngResource:

    ```
    angular.module('starter.services', ['ngResource'])
    ```

1. In that module, define a **service** named **Session** that uses the Angular resource module to provide access to the REST services at the specified endpoint:

    ```
    angular.module('starter.services', ['ngResource'])
    
    .factory('Session', function ($resource) {
        return $resource('http://localhost:5000/sessions/:sessionId');
    });
    ```
    
    > In a real-life application, you would typically externalize the server parameters in a config module.

1. The starter.services module you just created has a dependency on the Angular resource module which is
 not included by default. Open index.html and add a script tag to include **angular-resource.min.js** (right after 
 ionic-bundle.js):

    ```
    <script src="lib/ionic/js/angular/angular-resource.min.js"></script>
    ```

1. Add a script tag to include the **services.js** file you just created (right after app.js):

    ```
    <script src="js/services.js"></script>
    ```


<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="create-ionic-application.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 
Previous</a>
<a href="create-angular-controller.html" class="btn btn-default pull-right">Next <i class="glyphicon 
glyphicon-chevron-right"></i></a>
</div>
</div>



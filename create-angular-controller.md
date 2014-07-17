---
layout: module
title: Module 5&#58; Creating the Session Controllers
---
[AngularJS controllers](https://docs.angularjs.org/guide/controller) act as the glue between views and services. A controller often invokes a method in a service to get data that it stores in a [scope](https://docs.angularjs.org/guide/scope) variable so that it can be displayed by the view. In this module, you create two controllers: SessionsCtrl manages the session list view, 
and SessionCtrl manages the session details view.


## Step 1: Declare starter.services as a Dependency

The two controllers you create in this module use the **Session** service defined in the starter.services module. To 
add 
starter.services as a dependency to the starter.controller module: 

1. Open **conference/js/controllers.js**

1. Add **starter.services** as a dependency to make the Session service available to the controllers:

  ```
  angular.module('starter.controllers', ['starter.services'])
  ```

## Step 2: Implement the Session List Controller

1. In controllers.js, delete **PlayListsCtrl** (plural)

1. Replace it with a controller named **SessionsCtrl** that retrieves the list of conference sessions using the Session 
service and stores it in a scope variable named **sessions**:

    ```
    .controller('SessionsCtrl', function($scope, Session) {
        $scope.sessions = Session.query();
    })
    ```

## Step 3: Implement the Session Details Controller

1. In controllers.js, delete **PlayListCtrl** (singular)
 
1. Replace it with a controller named **SessionCtrl** that retrieves a specific session using the Session service and 
stores it in a scope variable named **session**:

    ```
    .controller('SessionCtrl', function($scope, $stateParams, Session) {
        $scope.session = Session.get({sessionId: $stateParams.sessionId});
    });
    
    ```



<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="create-angular-service.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 
Previous</a>
<a href="create-ionic-template.html" class="btn btn-default pull-right">Next <i class="glyphicon 
glyphicon-chevron-right"></i></a>
</div>
</div>



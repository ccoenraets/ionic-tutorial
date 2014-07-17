---
layout: module
title: Module 3&#58; Creating an Ionic Application
---
In this module, you use the Ionic CLI (command line interface) to create a new application based on the sidemenu 
starter app. 

## Steps

1. Open a new terminal window (Mac) or a command window (Windows), and navigate (cd) to the **ionic-tutorial** directory

1. Using the Ionic CLI, create an application named **conference** based on the **sidemenu** starter app:

  ```
  ionic start conference sidemenu
  ```

1. Run the application. Open a browser and access the following URL:

  [http://localhost:5000](http://localhost:5000)

  This works because the Node.js server is configured to serve static pages in the **conference/www** folder. Open 
  server.js in ionic-tutorial/server to see the code. Specifically:

  ```
  app.use(express.static('../conference/www'));
  ```

  > NOTE: Because of cross domain policy issues (specifically when loading templates), 
  you have to load the application from a server (using the http protocol and not the file protocol). If a local 
  server (like the Node server in this tutorial) is not available, you can also load your application using [ionic 
  serve](http://ionicframework.com/docs/guide/testing.html).


1. Back in the application, open the side menu ("hamburger" icon in the upper left corner) and select 
**Playlists**. 
Select a playlist in the list to see the details view (not much to see at this point).

  In the next modules, you will replace the playlists with a list of conference sessions retrieved from the 
  server using the REST services you experimented with in the previous module.

1. Open the side menu again and select **Login**. Click the Login button to close the window (Login is not 
implemented in the starter app).

    In the last module of this tutorial you will implement Login using Facebook.

<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="start-node-server.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 
Previous</a>
<a href="create-angular-service.html" class="btn btn-default pull-right">Next <i class="glyphicon 
glyphicon-chevron-right"></i></a>
</div>
</div>



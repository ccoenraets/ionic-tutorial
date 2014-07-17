---
layout: module
title: Module 2&#58; Starting the Node Server
---
In this module, you install and start a Node.js server that exposes the conference data (sessions and speakers) 
through a set of REST services.

## Steps

1. Download the supporting files for this tutorial [here](https://github.com/ccoenraets/ionic-tutorial/archive/master.zip), or clone the repository:

    ```
    git clone https://github.com/ccoenraets/ionic-tutorial
    ```

  > If you downloaded the zip file, unzip it anywhere on your file system.

1. Open a terminal window (Mac) or a command window (Windows), and navigate (cd) to the **ionic-tutorial/server** 
directory

1. Install the server dependencies:

  ```
  npm install
  ```

1. Start the server:

  ```
  node server
  ```
  
  > If you get an error, make sure you don't have another server listening on port 5000.

1. Test the REST services. Open a browser and access the following URLs:
  - [http://localhost:5000/sessions](http://localhost:5000/sessions) (for a list of conference sessions returned as a JSON document)
  - [http://localhost:5000/sessions/1](http://localhost:5000/sessions/1) (for information about a specific session )
  

<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="install-ionic.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 
Previous</a>
<a href="create-ionic-application.html" class="btn btn-default pull-right">Next <i class="glyphicon 
glyphicon-chevron-right"></i></a>
</div>
</div>



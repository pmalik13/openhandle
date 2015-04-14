# Introduction #

This page contains information about how to obtain the [OpenHandle source code](http://code.google.com/p/openhandle/source/checkout) and how to use it once you have obtained it.


# Checking out the source #

The latest version of the OpenHandle source is kept in the Google Code [Subversion](http://svnbook.red-bean.com/en/1.4/index.html) repository for this project. Details on how to check it out are provided on [the source page](http://code.google.com/p/openhandle/source/checkout).

# Compiling the source #

OpenHandle has a critical dependency on version 6 of the [Handle client for Java](http://handle.net/client_download.html) which is not distributed with OpenHandle. Additionally, OpenHandle itself uses the software project management tool [Maven](http://maven.apache.org/) to manage build and packaging. Therefore, to build OpenHandle, you need to complete the following steps:

  1. Checkout the latest version of the [OpenHandle source code](http://code.google.com/p/openhandle/source/checkout) to your preferred location (this will be the **project home**).
  1. If you have not already done so, download and install [Maven](http://maven.apache.org/download.html).
  1. Download the [Client Library (ver. 6) -- Java version](http://handle.net/client_download.html) and unpack the zip file, which contains the file `handle.jar`.
  1. To install `handle.jar` to your local Maven repository, at a command prompt type
```
mvn install:install-file -Dfile=<path-to-handle.jar> -DgroupId=net.handle -DartifactId=handle -Dversion=6 -Dpackaging=jar
```
  1. You can now build OpenHandle using [the normal Maven targets](http://maven.apache.org/plugins/index.html) by entering them at a command prompt in the **project home** root folder (i.e. the same folder where, once the source has been checked out, you have a file called `pom.xml`). Some examples:
    * `mvn clean` - delete all previously built files
    * `mvn compile` - compile the source
    * `mvn package` - create a war file of the OpenHandle application which can be deployed in your web app container of choice (e.g. [Tomcat](http://tomcat.apache.org) or [Jetty](http://www.mortbay.org/))
    * `mvn jar:jar` - compile the source and create a jar file of the OpenHandle code
    * `mvn tomcat:run` - run the OpenHandle web application in an embedded Tomcat server on your local machine
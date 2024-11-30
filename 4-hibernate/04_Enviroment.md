### Environment Setup

Downloading Hibernate  
It is assumed that you already have the latest version of Java installed on your system. Following are the simple steps to download and install Hibernate on your system −

Make a choice whether you want to install Hibernate on Windows, or Unix and then proceed to the next step to download .zip file for windows and .tz file for Unix.

Download the latest version of Hibernate from http://www.hibernate.org/downloads.

At the time of writing this tutorial, I downloaded hibernate-distribution 5.3.1.Final from mvnrepository and when you unzip the downloaded file, it will give you directory structure as shown in the following image

Installing Hibernate

Once you downloaded and unzipped the latest version of the Hibernate Installation file, you need to perform following two simple steps. Make sure you are setting your CLASSPATH variable properly otherwise you will face problem while compiling your application.

Now, copy all the library files from /lib into your CLASSPATH, and change your classpath variable to include all the JARs −

Finally, copy hibernate3.jar file into your CLASSPATH. This file lies in the root directory of the installation and is the primary JAR that Hibernate needs to do its work.

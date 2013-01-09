---
layout: jclouds
title: Developer Setup instructions for Eclipse
---

# jclouds Developer Setup instructions for Eclipse

## Introduction
The following document will help you get started, if you are unfamiliar with eclipse, maven, testng and git.

## Pre-requisites
*  Install Java 7
*  Install [Apache Maven 3.0.x](http://maven.apache.org/download.cgi)
*  Install [Git](http://git-scm.com/downloads)
*  Install [Eclipse 3.4 or higher](http://www.eclipse.org/downloads/)


## Setup Eclipse

### Setup m2eclipse plugin
1.  Open Eclipse
2.  Go to `Help -> Eclipse Marketplace`
3.  Find *m2eclipse*
4.  Choose `Maven Integration for Eclipse` by clicking the `Install` button

### Setup TestNG plugin
1.  Open Eclipse
2.  Go to `Help -> Eclipse Marketplace`
3.  Find **testng**
4.  Choose `TestNG for Eclipse` by clicking the `Install` button

## Clone jclouds from Github

* Go to [Github jclouds](https://github.com/jclouds/jclouds) and *fork* jclouds
* Clone your jclouds's fork by using something like:
	* `git clone https://github.com/username/jclouds.com.git` 
	* `cd jclouds && mvn eclipse:eclipse -Dmaven.javadoc.skip=true -DdownloadSources=true -DdownloadJavadocs=true`
  	 
## Import into Eclipse
1.  Open Eclipse
2.  `File -> Import … -> Maven -> Existing Maven projects
3.  Choose as Root directory your local git repository folder, ex. `/Users/username/projects/jclouds`
4.  Select all projects

# Running Tests

Tests are created in TestNG, so make sure you have the testNG plugin for Eclipse installed.  

## Live testing 

To run tests that use a real live service like aws-s3, you will need to provide your credentials to Eclipse and tell your tests to use them.  
You'll key these on the provider name (ex. provider  is aws-s3, cloudfiles-us, aws-ec2, etc)

To implement this, open the test's Run Configurations and enter in the following into VM arguments-

    -Dbasedir=. -Dtest.provider.identity=identity -Dtest.provider.credential=credential

ex. for **vcloud**

    -Dbasedir=. -Dtest.vcloud.endpoint=https://vcloudserverilike/api Dtest.vcloud.identity=user@org -Dtest.vcloud.credential=password

### Testing a BlobStore

If you are testing a BlobStore, you will also need to pass the test initializer you can find in its pom.xml file

ex. for *aws-s3*

    -Dbasedir=. -Dtest.aws-s3.identity=accesskey -Dtest.aws-s3.credential=secret -Dtest.initializers=org.jclouds.aws.s3.blobstore.integration.AWSS3TestInitializer

## Ssh testing

Ssh tests need access to an ssh host you have access to.  
Note that this is only required for running pure SSH tests.  
SSH indirectly used via a cloud will use the cloud credentials not the ones below. 
Note that the destination must be a Unix-like host that at least contains a world readable /etc/passwd file.

*  In Eclipse's Preferences open the section Run/Debug > String Substitution
*  Create two new variables named:
	*  `test.ssh.username` with the value of your ssh username
	*  `test.ssh.password` with the value of your ssh password

Then, for each test that uses ssh, open the test's Run Configurations and enter in the following into VM arguments:

    -Dtest.ssh.host=localhost -Dtest.ssh.port=22 -Dtest.ssh.username=${test.ssh.username} -Dtest.ssh.password=${test.ssh.password}

*  note that you can replace {{{test.ssh.host}}} and {{{test.ssh.port}}} above if you are not connecting to localhost.


## Create a demo project

For instance if you want to create a demo project in aws, you can do it with following steps:

*  Copy a sample project there (aws/demos directory) 
*  Change pom.xml from demos and add new module
*  Change pom.xml of the new project (change names and classes)
*  Add your code
*  `mvn eclipse:clean `
*  `mvn eclipse:eclipse -DskipTests=true`
*  You  can import your project now in eclipse.



## Clean build 
  * Close Eclipse
  * `mvn eclipse:clean`
  * `mvn clean` (may not be needed?)
  * `mvn install`
  * Then to start coding again:
    * `mvn eclipse:eclipse -Dmaven.javadoc.skip=true`
    * Open Eclipse
    * Delete old project references (but not underlying files)
    * Re-import projects

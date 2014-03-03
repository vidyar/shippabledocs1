:titles: Java buildsample
:description: A brief description about java buildsample
:keywords: Java, Language,jdk,script,Notification alerts


.. _java :

Java-buildsample 
===================

The goal of this code sample is to show how you can set up and run your repo in shippable platform.

A sample repository `Java-buildsample  <https://github.com/Shippable/Java-buildsample>`_ has been created using `cobertura <http://cobertura.github.io/cobertura/>`_ and `Junit <http://junit.org/>`_ . Keep the test and code coverage outputs in our special folders called Shippable/testresults and Shippable/codecoverage to get the reports parsed and the test results must be in Junit format.

We need the yml file to analyze the project details. So add the shippable.yml file to the root of your repo by specifying:

**Language :** Specify the language used to create the project. Java is used in our project.


**jdk :** Specify the jdk against which your build needs to run. Here we are using oraclejdk7, openjdk6 and openjdk7.


**script :** Specify the command to run the test and coverage using script key. We are mentioning the path of output file shippable/testresults and shippable/codecoverage in "build.properties" file for this project.


**Notification alerts:** Email notifications are added to get the alerts about the build status.

Here is the complete yml file for Java-buildsample:

.. code-block:: bash

	language: java
	jdk:
   	   - oraclejdk7
           - openjdk6
           - openjdk7
        script: 
           - ant coverage
        notifications:
          email:
              recipients:
         	 - exampleone@org.com
         	 - exampletwo@org.com

 

Create a project by enabling the repo Java-buildsample and run it using Ubuntu minion. Once the build finishes execution, you can check for the console output, test and codecoverage results on the respective builds page.

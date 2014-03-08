:title: java 
:description: How to configure shippable.yml file for java 
:keywords: yml,java,maven,oraclejdk6,openjdk6,oraclejdk7,openjdk7

.. _langjava :

java 
======
This section helps you to create a shippable.yml file for your Java project.

- Tell us what your build environment is. This is an option setting and if ommitted Ubuntu 12.04 is used as a default
    .. code-block:: python
        
        # Build Environment
        build_environment: ubuntu1204

- Set the appropriate language and the version number. You can test against Openjdk6, Openjdk7, Oraclejdk6 and Oraclejdk7 for a single push by adding more entries. Java minions use ``jdk`` by default to set the runtime platform.
	.. code-block:: python
	
     		# language setting
              language: java        
        	# jdk tag
	      jdk:
	       - openjdk7
	       - oraclejdk7
	       - openjdk6
	       - oraclejdk8

**Testing using java build tool**

**maven**

- If your repository root has pom.xml file, then our java builder will use Maven 3 to build it.By default it will run the test using **mvn test**.
	
- Java builder will execute the below line to install project dependencies with maven before it starts running test. 
      .. code-block:: python
	
	     mvn install -DskipTests=true

**Gradle**

- If your repository root has "build.gradle", then our java builder will use gradle to build it. By default it will use **gradle check** to run the test.

- Java builder will execute the below line to install the project dependencies with gradle.
      .. code-block:: python

	     gradle assemble	

**Ant**

- If your repository root does not have gradle ot maven files, then our java builder will use Ant to build it. By default it will use **Ant test** to run the test suite.

- Since there are no standard way to install ant dependencies, You need to specify the **install:** key to install your project dependencies with Ant.
       .. code-block:: python
           	
	       language: java
	       install: ant deps

Save the test outputs in shippable/testresults and codecoverage output in shippable/codecoverage folder to get the reports parsed. Otherwise you will not find the reports in our CI. 

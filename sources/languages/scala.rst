:title: scala 
:description: configuring yml file for scala
:keywords: scala,SBT,OracleJDK6,OracleJDK7,OpenJDK6,OpenJDK7 

.. _langscala:

scala 
======

This section helps you to build environment and configuration topics specific to Scala projects.

- Tell us what your build environment is. This is an option setting and if ommitted Ubuntu 12.04 is used as a default
    .. code-block:: python
        
        # Build Environment
        build_environment: ubuntu1204

- Set the appropriate languge and the version number. You can test against multiple version for a single push by adding more entries. Scala minions use ``scala`` by default to set the version.
  
- We support for SBT,oraclejdk6, oraclejdk7 ,openjdk6 and openjdk7 . Specify scala versions like 2.8.x, 2.9.x and 2.10.x in the shippable.yml file as shown below.
    .. code-block:: python
	
	language: scala
	scala:
   	   - 2.8.2
   	   - 2.9.2

- Scala builder assumes dependency management based on the projeccts like maven ,gradle or SBT, it will pull down project dependencies automatically before running tests.

- To test against multiple JDKs, use jdk tags. For example, to test against the oraclejdk6, oraclejdk7, openjdk6 and openjdk7
	.. code-block:: bash

	      jdk:
		- oraclejdk6
  		- oraclejdk7
  	        - openjdk6
		- openjdk7

**SBT projects :**

- If your repository root contains project directory or build.sbt file, then Scala builder will use sbt ++$SHIPPABLE_SCALA_VERSION test to run your test suite.
	   

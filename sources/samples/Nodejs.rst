:title:  Node.js-buildsample
:description:   A brief description about Nodejs-buildsample 
:keywords: Node.js, Language, version numbers, script, Notification alerts


.. _Nodejs :

Node.js-buildsample
======================

The goal of this code sample is to show how you can set up and run your repo in shippable platform.

A sample repository `Node.js-buildsample <https://github.com/Shippable/Node.js-buildsample>`_  has been created using the coverage tool `istanbul  <https://npmjs.org/package/istanbul>`_  and the testing tool `mocha  <https://npmjs.org/package/mocha>`_ .

Keep the test and code coverage outputs in our special folders called Shippable/testresults and Shippable/codecoverage to get the reports parsed.

We need the yml file to analyze the project details. So add the shippable.yml file to the root of your repo by specifying:

**Language :** Specify the language used to create the project.  We have used node_js in our project.

**version numbers :** 0.10 is used.

**after_script:** Specify the command to run the test and coverage and save the results in their respective shippable folders. If you have not created the folders, then you can create it using the before_script key.

**Notification alerts:**  Email notifications are added to get the alerts about the build status.

Here is the complete yml file for Node.js-buildsample.

.. code-block:: bash
	
	node_js:
           - "0.10"
        language: node_js
	before_script: mkdir -p shippable/codecoverage
	after_script: 
  	  - ./node_modules/.bin/istanbul cover ./node_modules/.bin/_mocha -- -u tdd 
  	  - ./node_modules/.bin/istanbul report cobertura --dir  shippable/codecoverage/
	notifications:
  	  email:
    		- exampleone@org.com

Enable the repo Node.js-buildsample and run it using Ubuntu minion. Once the build finishes execution, you can check for the console output, test and codecoverage results on the respective builds page.

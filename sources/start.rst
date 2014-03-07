:title: Getting Started
:description: Basic getting started section
:keywords: getting started, questions, documentation, shippable

.. _getstarted:

Getting Started
===============

**Step 1** : Sign Up
--------------------

Shippable uses Github as an auth provider and as a result you need a Github account to use us. Clicking on the Signin/Signup button will take you to Github app authorization page that requests you to give Read/Write permissions to your repos. You can authorize only for public repos or both private and public.

.. note::
    We request write permissions to add deploy keys to your repos so that webhooks work. We do not touch anything else in the repo.


Once you are authenticated by Github and redirected back to Shippable, and a wizard will get you setup and started with your Minion

-------

**Step 2** : Hook up Github
---------------------------

After signing in to Shippable, goto your repositories page by clicking on settings button. You will see a list of repositories for your personal accounts and any organizational accounts you have access to. Enable the repo that you wish to do build.


	
-------

**Step 3** : Create YML file
-----------------------------

Our CI environment needs a little information about your project to run the right build steps. We look for a file called ``shippable.yml`` in the root folder of your repo for this info. 

.. note::
  This example is for a node.js project. For other languages, refer to our :ref:`language guides <langrefs>`. 

  **If you use TravisCI,  we support** ``.travis.yml`` **natively, so that your test your projects in parallel with Shippable and compare the speed and rich visualizations.**

- Tell us what your build environment is. This is an option setting and if ommitted Ubuntu 12.04 is used as a default
    .. code-block:: python
        
        # Build Environment
        build_environment: Ubuntu 12.04

- Set the appropriate language and the version number. You can test against multiple version for a single push by adding more entries. We support all versions of Node.js as it is auto installed during your first run
    .. code-block:: python
        
        # language setting
        language: node_js

        # version numbers, testing against two versions of node
        node_js:
          - 0.10.25

- ``before_install`` tag can be used to install any additional libraries if needed. We use ``npm`` to run all dependencies in ``package.json``
    .. code-block:: python
        
        # npm install runs by default but shown here for illustrative purposes
        before_install: 
         - npm install docco
         - npm install coffee-script

- ``script`` tag can be used to install any additional libraries if needed. We use ``npm`` to run all dependencies in ``package.json``
    .. code-block:: python
        
        # Running npm test to run your test cases
        script: 
         - npm test

**For full documentation of YML refer** :ref:`HERE <setup>`.

--------

**Step 4** : Test Visualizations
--------------------------------

If you would like to use our test visualization feature, then the code coverage output should be in cobertura xml format and test result should be in junit format. Refer Code Sample for more details.


--------

**Step 5** : Run the build
---------------------------

Builds can be triggered manually through Shippable.com. 

- Go to builds page and select the project from the list on the sidebar 
- Click on the Run button. As soon as you click on the run button, console log from the minion starts to stream to your browser through sockets. Make sure you have enough minions to run the build by going to the minions page.

.. note::

  If your project has multiple versions, then each version results in a separate build.You can check the console output for each build by clicking on the build numbers listed in the latest build tab.



Alternatively, you can also use webhooks to trigger your build. Webhooks are user-defined HTTP callbacks. They are usually triggered by some event, such as pushing code to a repository. Your builds will run automatically when someone pushes in code into the repository or ask for a pull request. Further details are here.

.. note::

	Minions are container used to run your builds. Provision a minion by clicking  on **+**  in minions page to add ubuntu server 12.04 lts to run and test your builds.

--------

**Step 6** : Check output
------------------------- 
 
In addition to running builds, Shippable also provides visualization of key information of every build. Following are the reports available for every build

**Console Log** :
Stdout of a build run is streamed to the browser in realtime using websockets. In addition, there are other important pieces of information like 

* build status
* duration
* github changeset id
* committer info

**Artifact archive** :
Upon completion of the build, build artifacts automatically archived for each run. This is available to download as a .tar file from the build page. All files in ./shippable folder at the root of the project is automatically archived.

**Test cases** :
Test run output is streamed to console log at realtime. If you would like to use Shippable parser to parse test output and provide a graphical representation, export a JUNIT xml of your test output to ./shippable/testresults folder. After the build completes, our build engine will automatically parse it and results appear on the test tab.

**Code Coverage** :
Just having tests executing but not really knowing what percentage of your code is actually being tested is like "Flying a plane without GPS". A variety of coverage tools like opencover, cobertura etc. provide a way to measure coverage of your tests. Output of these tools can be exported to ./shippable/codecoverage and our build engine will automatically parse it and results appear on the coverga tab.

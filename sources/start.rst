:title: Getting Started Test
:description: Basic getting started section
:keywords: getting started, questions, documentation, shippable

.. _getstarted:

Getting Started
===============

**Step 1** : Sign Up
--------------------

Shippable uses Github as an auth provider, so you need a Github account to use us. Clicking on the 'Sign in' or 'Sign up' buttons will take you to the standard Github app authorization page and requests you to give Shippable Read/Write permissions to your repos. You can choose to authorize us only for public repos or both private and public repos.

.. note::
    We realize that most people do not want to give write access to their repo. However, we need write permissions to add deploy keys to your repos so that webhooks work. We do not touch anything else in the repo.

After authorization, you will be authenticated by Github and redirected back to Shippable. An Ubuntu 12.04LTS Minion will be automatically provisioned for you. You are now ready to set up CI! 

-------

**Step 2** : Enable CI for Github repos
---------------------------

You can now go to your repositories page by clicking on Settings in the top menu. You will see a list of repositories for your personal accounts and any organizational accounts you have access to. Enable the repo that you wish to do build.

-------

**Step 3** : Create YML file
-----------------------------

Our CI environment needs a little information about your project to run the right build steps. We look for a file called ``shippable.yml`` in the root folder of your repo for this info. 

.. note::
  This example is for a node.js project. For other languages, refer to our :ref:`language guides <langrefs>`. 

  **If you use TravisCI,  we support** ``.travis.yml`` **natively, so that you can test your repos in parallel with Shippable and compare the speed and rich visualizations.**

- **Tell us what your build environment is. This is an optional setting and if omitted, Ubuntu 12.04 is used as a default.**
    .. code-block:: python
        
        # Build Environment
        build_environment: Ubuntu 12.04

- Set the appropriate language and the version number. You can test against multiple versions for a single push by adding more entries. We support all versions of Node.js as it is auto installed during your first run.
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

If you would like to use our test visualization feature, then your code coverage output should be in cobertura xml format and test result should be in junit format. Refer :ref:`Code Samples <samplesref>` for more details. 


--------

**Step 5** : Run the build
---------------------------

Builds can be triggered through webhooks or manually through Shippable.com. 

Webhooks -
Webhooks are user-defined HTTP callbacks. They are usually triggered by some event, such as pushing code to a repository or creating a pull request. Your builds will run automatically when webhooka are triggered. Further details are here.

Manual Builds - 

- Select Builds from the top menu and then select the project from the list in the sidebar to the left. 
- Click on the Run button. Immediately, the console log from your build minion starts to stream to your browser through sockets. If your build does not start or get queued, make sure you have enough minions to run the build by going to the minions page.

.. note::

  If your project has multiple versions, then each version results in a separate build.You can check the console output for each build by clicking on the build numbers listed in the latest build tab.

--------

**Step 6** : Check output
------------------------- 
 
In addition to running builds, Shippable also provides visualization of key information for every build. 

The following information is available for every build -

**Console Log** :
Stdout of a build run is streamed to the browser in real-time using websockets. In addition, there are other important pieces of information like 

* build status
* duration
* github changeset id
* committer info

**Artifact archive** :
Upon completion of the build, build artifacts are automatically archived for each run. You can open the build details tab by clicking on a build number and then download artifacts as a .tar file. All files in ./shippable folder at the root of the project are automatically archived.

**Test cases** :
Test run output is streamed real-time to the console log when the tests are executed. If you want Shippable's parser to parse test output and provide a graphical representation, you need to export a JUNIT xml of your test output to the ./shippable/testresults folder. After the build completes, our build engine will automatically parse it and results appear on the Tests tab (available in the set of tabs to the right of the build details page).

**Code Coverage** :
Executing tests but not really knowing what percentage of your code is actually being tested is like "Flying a plane without GPS". A variety of coverage tools like opencover, cobertura etc. provide a way to measure coverage of your tests. You can export the output of these tools to ./shippable/codecoverage and our build engine will automatically parse it and the results will appear on the Coverage tab.

:title: Python-buildsample
:description: A brief description about Python-buildsample
:keywords: Python, Language,version number, Install, Notification alerts

.. _python :

Python-buildsample
==================== 

The goal of this code sample is to show how you can set up and run your repo in shippable platform.

A sample repository `Python-buildsample <https://github.com/Shippable/Python-buildsample>`_ has been created using `nose <https://pypi.python.org/pypi/nose>`_ and `coverage 3.7  <https://pypi.python.org/pypi/coverage/>`_  .


Keep the output generated for test and code coverage in our special folders called Shippable/testresults and Shippable/codecoverage to get the reports parsed and to get the visualization of the reports.The test results must be generated in Junit format.

We need the yml file to analyze the project details. Add the shippable.yml file to the root of your repo by specifying:


**Language :** Specify the language used to create the project. Python is used in this sample project.

**version number :** Specify the version numbers against which your build needs to run. We are using "2.7".

**Install :** Install the required dependencies for your repo here.

**script :** Specify the command to run the test and coverage and save the results in their respective 
shippable folders. If you have not created the folders, then you can create it using the before_script key.

**Notification alerts:**  Email notifications are added to get the alerts about the build status.

Here is the complete yml file for Python-buildsample

.. code-block:: bash
    
    python:
  	- "2.7"
    language: python
    before_script: mkdir -p shippable/codecoverage shippable/testresults
    install: pip install --use-mirrors -r requirements.txt
    script: 
      - nosetests python/sample.py --with-xunit --xunit-file=shippable/testresults/nosetests.xml
      - coverage run --branch python/sample.py
      - coverage xml -o shippable/codecoverage/coverage.xml python/sample.py
    notifications:
      email:
       - exampleone@org.com

Enable the repo Python-buildsample and run it using Ubuntu minion. Once the build finishes execution, you can check for the console output, test and codecoverage results on the respective builds page.


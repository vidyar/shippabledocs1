:title: Ruby Sample
:description: Step by step instructions to configure a ruby build
:keywords: Ruby, Language, version number,env, Notification alerts

.. _ruby:

Ruby-buildsample
==================

The goal of this code sample is to show how you can set up and run your repo in shippable platform.

A sample repository `Ruby-buildsample <https://github.com/Shippable/Ruby-buildsample>`_  has been created using `simplecov <http://rubydoc.info/gems/simplecov/>`_  and `RSpec <http://rspec.info/>`_  .

We need the yml file to analyze the project details. So add the shippable.yml file to the root of your repo by specifying the following details:

**Language :** Specify the language used to create the project. Ruby is used in this sample code.


**version number :** Specify the platform version using **rvm:**. we are using 1.9.2 in our buildsample project .

**env:** Keep the test and code coverage outputs in our special folders called Shippable/testresults and Shippable/codecoverage to get the reports parsed.

**Notification alerts:** Email notifications are added to get the alerts about the build status.

Here is the complete yml file for Ruby-buildsample:

.. code-block:: bash
	
    language: ruby
    rvm:
      - 1.9.2
    env:
      - CI_REPORTS=shippable/testresults COVERAGE_REPORTS=shippable/codecoverage
    notifications:
    email:
      - exampleone@org.com



Enable the repo Ruby-buildsample and run it using Ubuntu minion. Once the build finishes execution, you can check for the console output, test and codecoverage results on the respective builds page.

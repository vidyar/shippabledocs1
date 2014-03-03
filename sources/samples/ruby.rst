:title: Ruby Sample
:description: Step by step instructions to configure a ruby build
:keywords: Ruby, Language, version number,env, Notification alerts

.. _Ruby_buildsample :

Ruby-buildsample
==================


A sample repository `Ruby-buildsample <https://github.com/Shippable/Ruby-buildsample>`_  has been created using `simplecov <http://rubydoc.info/gems/simplecov/>`_  and `RSpec <http://rspec.info/>`_  .

We need the yml file to analyze your project details . So add the shippable.yml file to the root of your repo by specifying :

**Language :** ruby is used

**version number :** 1.9.2 is used. Specify it using rvm: .

**env:** Keep the test and code coverage outputs in our special folders called Shippable/testresults and Shippable/codecoverage to get the reports parsed.

**Notification alerts:** Email notifications are added to get the alerts about the build status.


Create a job by selecting the repo Ruby-buildsample and run it using Ubuntu 12.04LTS node. Once the build finishes execution, you can check for the console output, test and codecoverage results as well as the build trend graphs.

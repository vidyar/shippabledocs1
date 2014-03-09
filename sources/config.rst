:title: Shippable Configuration
:description: Complete guide to how Shippable is setup
:keywords: shippable, signup, login, registration

.. _setup:

A bit deeper
============

Build Minions and Build configuration are two things that you should care the most about when using Shippable. The sections below talk about these in greater detail.


**Minions**
-----------

Minions are Docker based containers that run your builds and tests. You can think of them as Build VMs. Each minion runs one build at a time, so the more minions you have, the more concurrency you will get.  

We automatically create one minion for you when you first sign in to Shippable. Depending on your subscription, you can create more minions by going to Settings->Minions and clicking on the + sign.

Each minion starts from a base image and can be customized by specifying ``before_install`` scripts in the YML file. A minion can be configured to run any package, library, or service that your application requires. There are some preinstalled tools and services that you can use to customize your minions even further. 

Operating Systems
.................

All our Linux minions start from a vanilla base image from the Docker registry. We support all images as a starting point for your minion. Minions can be further customized by using the ``before_install`` and ``install`` tags in ``shippable.yml`` that is in the root of your code repository.

(Coming soon) Our Windows minions are based on AWS AMI for Windows 2012.

State Management
................

Shippable maintains the state of each minion between builds. We believe that build speed is very important, so reinstalling everything and cloning git repos every single time just doesn't make sense. 

(Coming soon) However, we also understand the value of testing on pristine environments. Hence we have a commit message tag [reset minion] which will reset your minion to its base setting and allow your test to run on a prisitne minion.


Common Tools
............

A set of common tools are available on all minions. The following is a list of available tools -

- Latest release of Git repository
- apt installer
- Networking tools  
  
  - curl
  - wget
  - OpenSSL

- At least 1 version of (check out the language documentation for specific versions)
  
  - Ruby
  - Node
  - Python 
  - OpenJDK

- Services
  
  - MySQL
  - Postgre
  - SQLLite
  - MongoDB
  - Redis
  - ElasticSearch
  - Selenium Server

- Headless browser testing tools

  - xfvb
  - PhontomJS

- Libraries

  - ImageMagik
  - OpenSSL

- JDK Versions

  - Oracle JDK 7u6 (oraclejdk7)
  - OpenJDK 7 (alias: openjdk7)
  - OpenJDK 6 (openjdk6)
  - Oracle JDK 8 EA (oraclejdk8)

- Build Tools

  - Maven 3
  - Gradle 1.9
  - Make
  - SBT 0.12.1

- Preinstalled PIP Packages

  - mock
  - nosetests
  - py.test

- Gems

  - Bundler
  - Rake

----------

**Configuration**
------------------

This section is generic to all build environments and all languages. If you are looking for language specific tags, please refer to our language guides for more information.

``shippable.yml``
.................

We believe that developers need complete control of their build configuration. We also realize that most developers don't want to log into a UI to make changes every single time. 

While mulling over the best way to give developers control without asking them to go through our UI, we came across ``.travis.yml``, an open source initiative that created the basic framework for this very problem. Following the same paradigm, we ask you to have ``shippable.yml`` in the root of the repository you want to build. The structure of shippable.yml closely mimics travis since we see no reason to reinvent the wheel. We do have additional tags for added functionality, and these will become more numerous as we evolve Shippable. 

Since shippable.yml is a superset of ``.travis.yml`` , we support ``.travis.yml`` natively as well. So if you have a travis.yml in the root of your repo, we will read the config and set up your CI.

At a minimum, Shippable needs to have your language and build version specified in the yml. We will then default to the most common commands.

Build Flow
..........

When we receive a build trigger through a webhook or manual run, we execute the following steps - 

1. Clone/Pull the project from Github. This depends on whether the minion is in pristine state or not
2. ``cd`` into the workspace
3. Checkout the commit that is getting built
4. Run the ``before_install`` section. This is typically used to prep your minion and update any packages
5. Run ``install`` section to install any project specific libraries or packages
6. Run ``before_script`` section to create any folders and unzip files that might be needed for testing. Some users also restore DBs etc. here
7. Run the ``script`` command which runs build and all your tests
8. Run either ``after_success`` or ``after_failure`` commands
9. Run ``after_script`` commands

The outcome of all the steps upto 7 determine the outcome of the build status. They need to return an exit code of ``0`` to be marked as success. Everything else is treated as a failure.


----------

**Other useful configs**
------------------------

Shippable uses Docker containers to provide your with isolation and a dedicated build environment. Our command sessions are not sticky throughout the build, but they are sticky within a section of the build. For e.g. ``cd`` is sticky within ``before_script`` tag of ``shippable.yml``

script
......
You can run any script file as part of your configuration, as long as it has a valid shebang command and the right ``chmod`` permissions. 

.. code-block:: python
        
        # script file 
        script: ./minions/do_something.sh 



command collections
...................
``shippable.yml`` supports collections under each tag. This is nothing more than YML functionality and we will run it one command at a time.

.. code-block:: python
        
  # collection scripts 
  script: 
   - ./minions/do_something.sh 
   - ./minions/do_something_else.sh 

In the example above, our minions will run ``./minions/do_something.sh`` and then run ``./minions/do_something-else.sh``. The only requirement is that all of these operations return a ``0`` exit code. Else the build will fail.


git submodules
..............
Shippable supports git submodules. This is a cool functionality of breaking your projects down into manageable chunks. We automatically initialize ``.gitmodules`` file in the root of the repo. 

.. note::

  If you are using private repos, add the deploy keys so that our minion ssh keys are allowed to pull from the repo. This can be done through shippable.com

If its your own public repos then do this

.. code-block:: python
        
  # for public modules use
  git://github.com/someuser/somelibrary.git

  # for private modules you can use
  git@github.com:someuser/somelibrary.git

If you would like to turn submodules off completely use this

.. code-block:: python
        
  # for public modules use
  git:
   submodules: false


environment variables
.....................

We believe this is the most powerful feature of our platform. You can test your projects with multiple different setting for every push into your repo. Every statement of this command will trigger a seperate build with that version of the env variables. 

.. code-block:: python
        
  # environment variable
  env:
   - FOO=foo BAR=bar
   - FOO=bar BAR=foo


.. note::

  Env variables can create exponential number of builds when comined with ``jdk`` & ``rvm, node_js etc.`` i.e. its multiplicative

In this setting **4 builds** are triggered

.. code-block:: python
        
  # nPn builds
  node_js:
    - 0.10.24
    - 0.8.14
  env:
    - FOO=foo BAR=bar
    - FOO=bar BAR=foo


include & exclude branches
..........................

You can only build specific branches or exclude them if you choose to do so. 

.. code-block:: python

  # exclude
  branches:
    except:
      - test1
      - experiment2

  # include
  branches:
    only:
      - stage
      - prod


build matrix
............

This is by far the most powerful feature that Shippable has to offer. You can trigger multiple different test passes for a single code push. You might want to test agaisnt different versions of ruby, or different aspect ratio for your Selenium tests or best yet, just different jdk versions. You can so it all with our matirx build mechanism

.. code-block:: python

  rvm:
    - 1.8.7 # (current default)
    - 1.9.2
    - 1.9.3
    - rbx
    - jruby
    - ruby-head
    - ree
  gemfile:
    - gemfiles/Gemfile.rails-2.3.x
    - gemfiles/Gemfile.rails-3.0.x
    - gemfiles/Gemfile.rails-3.1.x
    - gemfiles/Gemfile.rails-edge
  env:
    - ISOLATED=true
    - ISOLATED=false

The above example will fire 36 different builds for each psu. Whoa! Need more Minions?



----------

**Services**
-----------------
Shippable offers a host of pre-installed services to make it easy to run your builds. In addition to these you can install other services also by using ``install`` tag of ``shippable.yml``. 

All the services are turned off by default and can be turned on by using ``services:`` tag.

MongoDB
.......
.. code-block:: bash
  
  # Mongo binds to 127.0.0.1 by default
  services:
   - mongodb

Sample Python code using `MongoDB <https://github.com/Shippable/mongodb-buildsample>`_.


MySQL
.....

.. code-block:: bash
  
  # MySQL binds to 127.0.0.1 by default and is started. default username is shippable with no password
  # Create a DB as part of before script to use it

  before_script:
      - mysql -e 'create database myapp_test;'
                                 
Sample Python code using `MySQL <https://github.com/Shippable/mysql-buildsample>`_.


PostgreSQL
..........

.. code-block:: bash

  # Postgre binds to 127.0.0.1 by default and is started. default username is "postgres" with no password
  # Create a DB as part of before script to use it

  before_script:
    - psql -c 'create database myapp_test;' -U postgres

Sample Python code using `PostgreSQL <https://github.com/Shippable/postgresql-buildsample>`_.


SQLite3
.......

SQLite is a software library that implements a self-contained, serverless, zero-configuration, transactional SQL database engine. So you can use SQLite, if you do not want to test your code behaviour with other databases.

Sample Python code using `SQLite <https://github.com/Shippable/sqlite-buildsample>`_.


Elastic Search
..............

.. code-block:: bash

  #elastic search is on default port 9200
  services:
      - elasticsearch

Sample Python code using `Elastic Search <https://github.com/Shippable/Elasticsearch-buildsample>`_.

Memcache
........

.. code-block:: bash

  #memcache runs on default port 11211
  services:
      - memcached

Sample Python code using `Memcache <https://github.com/Shippable/Memcache-buildsample>`_.


Redis
.....

.. code-block:: bash

  #redis runs on default port 6379
  services:
      - redis


Sample Python code using `Redis <https://github.com/Shippable/Redis-buildsample>`_.


----------

**Notifications**
-----------------
Shippable can notify you about your build status. If you want to get notified about the status of the builds like success, failure or unstable, then follow the rules below to configure your yml file. We will send the consolidated build reports in individual emails for matrix build projects. By default we will send the email notifications to the last committer.


Email notification
..................


You can configure the email notification by specifying the recipients id in ``shippable.yml`` file.

.. code-block:: bash

  notifications:
      email:
          - exampleone@org.com
          - exampletwo@org.com


You can also specify when you want to get notified using change|always|never. Always and never means you should be notified always or never.
change means you want to notify only when the build status changes on the given branch.

.. code-block:: bash
 
  notifications:
       email:
           recipients:
               - exampleone@org.com
               - exampletwo@org.com
           on_success: change
           on_failure: always


If you do not want to get notified, then you can configure the email notification to false.

.. code-block:: bash

  notifications:
     email: false


----------

**Continuous deployment**
-------------------------

Continuous deployment to Heroku
................................

Heroku supports Ruby, Node.js, Python, so you can use these languages to build and deploy apps on Heroku. You can deploy to your own server by adding the custom after_success:. For this you need to add the Public key that was generated for your subscription in shippable to set up continous deployment on providers.

* Go to settings and copy the SSH Key or public key generated for your subscription.
* Log In to Heroku and add the SSH key to your account 


A Sample deployment configurations to your shippable.yml file is given below

.. code-block:: bash

  after_success :
    - git push  git@heroku.com:shroudd-headland-1758.git master


You need to copy the Git URL from your project for deployment in heroku .

* Go to apps and select your project
* Go to the settings page of your project and copy the Git URL
* Add it to the shippable.yml file

.. code-block :: bash

  after_success :
    - git push git@heroku.com:shroudd-headland-1758.git master


----------

**Pull Request**
----------------


Shippable will integrate with github to show pull request status on CI. Whenever a pull request is opened for your repo, we will run the build for the respective pull request and let you know about the status. You can decide whether to merge the request or not based on the status shown on our CI. If you are accepting the pull request, then we will run one more build for the merged repo and we will send email notifications for the merged repo.

 
--------

**Collaborators**
------------------

Shippable will automatically add your github collaborators when you create a project and by default we will assign them the role **Build engineer**. You can see the list of collaborators or change there role by expanding your repo on settings page.


There are two types of roles that users can belong to.

**Owner :** 
Owner is the highest role. This role permits users to create, run and to delete a project. Owners can also manage permissions and even create other co-owners.


**Build engineer :** 
Build engineer can run or manage projects that are already setup. They have full visibility into the project and can trigger the build.


--------

**Build Termination**
-----------------------


If your script or test suite hangs for a long time or there hasn't been any log output in 10 minutes, then we will forcefully terminate the build by adding a message on console log.


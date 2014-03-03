:title: Ruby
:description: Language specific guide for Ruby
:keywords: containers, lxc, concepts, explanation, image, minion, shippable, Ruby

.. _ruby:

Ruby
====

This section helps you to build environment and configuration topics specific to Ruby projects.

**Ruby versions for testing** :

- Tell us what your build environment is. This is an option setting and if ommitted Ubuntu 12.04 is used as a default
    .. code-block:: python
        
        # Build Environment
        build_environment: ubuntu1204

- Set the appropriate languge and the version number. You can test against multiple version for a single push by adding more entries. Ruby minions use ``rvm`` by default to set the version
    .. code-block:: python
        
        # language setting
        language: ruby

        # version numbers, testing against two versions of node
        rvm:
         - 2.0.0
         - 1.9.3
         - 2.1.0
         - jruby-18mode
         - jruby-19mode
         - rbx
         - ruby-head
         - jruby-head
         - ree
	
.. note::
 We are setting multiple versions of Ruby here which means a single push to repo will trigger multiple builds 

- Though we pre-install a few version of Ruby, all the ruby versions are supported. As long as they're available as a binary for Ubuntu 12.04, you can specify custom patchlevels
    .. code-block:: python
        
        language: ruby
        rvm: 2.0.0-p247

.. note::
 This binds you to potentially unsupported releases of Rubies. It also extends your build time as downloading and installing a custom Ruby can add an extra 60 seconds or more to your build the first time it installs.

We are using Bundler, ``bundle install`` to install all your gems. We also use ``rake`` by default to run your build and hence you need to specify it in your gemdile

- If you are using a custom gemfile thats not in default location you can specify it with ``gemfile`` tag
    .. code-block:: python
        
        # gemfile tag
        gemfile: gemfiles/Gemfile.ci

.. note::
 If you give multiple gemfiles in the above tag, a matrix build is triggered for every version of the gemfile


- Additional arguments can be added to ``bundle install`` command and we will append them to the default command
    .. code-block:: python
        
        # bundle_args 
        bundler_args: --binstubs

- If you want to run some other commands before install you can do this
    .. code-block:: python
        
        # before_install tag
        before_install: gem install bundler --pre

- You could also set multiple environment variables and test against multiple different version by using the env variable in your code. This will fire 3 different builds, one for each env variable
    .. code-block:: python
        
        # env tag
		env:
		 - CHEF_VERSION=0.9.18
		 - CHEF_VERSION=0.10.2
		 - CHEF_VERSION=0.10.4

- You could also test against multiple ``jdk`` versions
    .. code-block:: python
        
        # jdk tag
		jdk:
		 - openjdk7
		 - oraclejdk7
		 - openjdk6

- One can also update the versions on your minion by running a simple command or even downgrade if you choose to. The below script will upgrade and downgrade as a demonstration
    .. code-block:: python
        
		before_install:
		 - gem update --system
  		 - gem --version
		 - gem update --system 2.1.11
  		 - gem --version

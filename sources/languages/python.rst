:title: python 
:description: Configuring yml file for python
:keywords: Python, pip, nosetests, mirrors

.. _langpython:

Python
======

This section helps you to configure the yml file for your python project.

**Choosing Python versions to test against**

- Tell us what your build environment is. This is an option setting and if ommitted Ubuntu 12.04 is used as a default.
    .. code-block:: bash
    
        # Build Environment
              build_environment: ubuntu1204

**Python versions for testing** :

- Set the appropriate language and the version number. You can test against multiple version for a single push by adding more entries. Python minions use ``python`` by default to set the version.
      .. code-block:: python
        
          # language setting
             language: python

          # version numbers, testing against two versions of node
            python:
             - "2.6"
             - "2.7"
             - "3.2"
             - "3.3"

- If you want to install dependecies for your project
	.. code-block:: python

	     install: "pip install -r requirements.txt --use-mirrors"

**Test Scripts**

- Python projects need to provide **script** key in their shippable.yml to specify what command to run tests with.
	.. code-block:: python

		# command to run tests
		script: nosetests

**Pre-installed packages**:


- Few packages like pytest, nose and mock are pre-installed in each virtualenv by default to ease running tests.

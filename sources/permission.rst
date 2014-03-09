:title: Permission management 
:description: Managing permissions on Shippable CI projects
:keywords: containers, lxc, docker, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, setup, github, permissions, teams

.. _permissions:

Permission Management
=====================

Shippable allows you to add github repository collaborators to your Shippable project and allow them to run builds.


Steps
.....

1. Click on subs on the sidebar on the left.

3. Pick the subscription under which your repo is enabled.

4. Click on the permissions icon to activate the permission management tab.

5. You will see a list of your github collaborators and can setup the right permissions for them.

.. note::

   Deleting collaborators does not automatically delete their Shippable access. You have to delete them from both places


Roles
.....

There are three types of roles that users can have -

**Owners :** 
Owner is the highest role. This role permits users to create, run and delete a job. Owners can also manage permissions and create other co-owners.


**Build Admins :** 
Build Admins can run or manage projects that are already setup. They have full visibility into the project and have notifications enabled by default.

**Members :** 
Members can only view the projects, console output and the reports. Members do not get auto-notified unless they are explicitly configured in the YML file.


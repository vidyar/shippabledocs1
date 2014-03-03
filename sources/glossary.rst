:title: Glossary
:description: Definitions of terms used in Shippable documentation
:keywords: concepts, documentation, shippable, CI/CD

.. _glossary:

Glossary
========

Definitions of terms used in Shippable documentation


**Subscriptions**
-----------------
In order Shippable to group CI projects under a common entity, a default subscription is created for every user account. A default free subscription is created on signup with quotas as per our pricing plans. Users can upgrade this to a paid plan and receive higher quotas.


**Minions**
-----------
Minions are containers that will run your builds and tests. More the number of minions you have, more parallel builds can run.  Each minion starts from a base image and can be further customized by using before_instal scripts in the YML file


**Projects**
------------
A Project is used to handle Builds/CI/CD on your repo. You need to create a project for each repo you want CI enabled. Every project is under a subscription and your limits/quotas are tracked at the subscription level
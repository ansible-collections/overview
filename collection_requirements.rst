*****************************
Ansible Collections Checklist
*****************************

.. contents:: Topics

Overview
========

**Warning:** Subject to frequent updates
       This is a "living document", expect it to change as we progress with the Collections work over the next few months.

Feedback
========

As with any project it's very important that we get feedback from users, contributors and maintainers. We recognize that the move to Collections is a big change in how Ansible is developed and delivered.

Please raise feedback by

* Discussing in ``#ansible-community`` in Freenode IRC
* Adding to the `Community Working Group IRC meeting <https://github.com/ansible/community/issues/539>`_
* Creating `GitHub Issues <https://github.com/ansible-collections/overview/issues>`_ against this repo


Why is this needed
===================


Repo structure
===============

galaxy.yml
----------

* tags MUST set
* dependencies MUST be set to ``'>=1.0.0'`` (Might have to think about dependency order when publishing)

meta/runtime.yml
----------------
Example: `meta/runtime.yml <https://github.com/ansible-collections/collection_template/blob/main/meta/runtime.yml>`_
* MUST define the minimim version of Ansible which this collection works with
  * If the Collection works with Ansible 2.9, then this shuld be set to

Modules & Plugins
------------------

Documentation
~~~~~~~~~~~~~~
All module and plugin ``DOCUMENTATION`` MUST

* Use the FQCN for ``M(...)`` FIXME Docs link
* Use the FQCN for ``SEEALSO`` FIXME Docs link
* Use the FQCN for ``extends_documentation_fragment:``, unles you are referring to doc_fragments from ansible-base

Contributor Workflow
====================

Changelogs
~~~~~~~~~~

FIXME: What's the actual minimum requirement here, is it a file called X?

FIXME: Add details and examples of automated system

Versioning and deprecation
~~~~~~~~~~~~~~~~~~~~~~~~~~

FIXME Details here
* Deprecation vs Collection version
* How many Collection major versions to give

Repo management
===============

Repo name
~~~~~~~~~

For collections under ansible-collections the repo SHOULD be named ``NAMESPACE.COLLECTION``

Branch name and configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All new repos under `ansible-collections<https://github.com/ansible-collections>`_ MUST have `main`` as the default branch.

Existing repos SHOULD be converted to use ``main``

Repo Protections:
* Allow merge commits: disallowed

Branch protections MUST be enforced
* Require linear history
* Include administrators

CI Testing
===========

At a minimim ``ansible-test sanity`` MUST be run from the ``ansible-base:stable-2.10`` branch

For most repos GitHub actions are sufficient, see `example<https://github.com/ansible-collections/collection_template/tree/main/.github/workflows>`_





Collections and Working Groups
==============================

* Issue for agenda (or pinboard if there aren't regular meetings) as pinned issue in the Repo


Other things
============
* ansible-base's runtime.yml


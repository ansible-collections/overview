*****************************
Ansible Collections Checklist
*****************************

.. contents:: Topics

Overview
========
This document is for maintainers of collections to provide them help, advice, and guidance on making sure their collections are correct.
**Warning:** Subject to frequent updates
       This is a "living document", expect it to change as we progress with the Collections work over the next few months.

Feedback
========

As with any project it's very important that we get feedback from users, contributors and maintainers. We recognize that the move to Collections is a big change in how Ansible is developed and delivered.

Please raise feedback by

* Discussing in ``#ansible-community`` in Freenode IRC
* Adding to the `Community Working Group IRC meeting <https://github.com/ansible/community/issues/539>`_
* Creating `GitHub Issues <https://github.com/ansible-collections/overview/issues>`_ against this repository

Keeping informed
================

Be sure you're subscribed to:
* `Changes impacting Collections <https://github.com/ansible-collections/overview/issues/45>` to track changes that Collection maintainers should be aware of 
* The Bullhorn, a newsletter for the Ansible developer community, `back issues and how to add content <https://github.com/ansible/community/issues/546>`_

Why is this needed
===================


Repo structure
===============

galaxy.yml
----------

* tags MUST be set
* dependencies MUST be set to ``'>=1.0.0'`` (Might have to think about dependency order when publishing)

meta/runtime.yml
----------------
Example: `meta/runtime.yml <https://github.com/ansible-collections/collection_template/blob/main/meta/runtime.yml>`_
* MUST define the minimum version of Ansible which this collection works with
  * If the collection works with Ansible 2.9, then this should be set to `>=2.9.10`
  * It's usually better to avoid adding `<2.11` as a restriction, since this for example makes it impossible to use the collection with the current ansible-base devel branch (which has version 2.11.0.dev0)

Modules & Plugins
------------------

Documentation
~~~~~~~~~~~~~~
All module and plugin ``DOCUMENTATION`` and ``RETURN`` MUST:

* Use the FQCN for ``M(...)`` and ``- module:`` references of ``seealso`` subsections. See `Linking within module documentation<https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_documenting.html#linking-within-module-documentation>`_
* Use collection version numbers for `version_added` (otherwise `version_added_collection` must be provided, but this is discouraged)

All module and plugin ``EXAMPLES`` MUST:

* Use FQCN for module (or plugin) name.
* For modules (or plugins) left in ansible-base use ``ansible.builtin.`` as a FQCN prefix, for example, ``ansible.builtin.template``

Other items:
* You MUST Use the FQCN for ``extends_documentation_fragment:``, unless you are referring to doc_fragments from ansible-base


Contributor Workflow
====================

Changelogs
~~~~~~~~~~

To give a consistent feel for changelogs across collections, and ensure for collections included in the ``ansible`` package we suggest you use `antsibull-changelog <https://github.com/ansible-community/antsibull-changelog>`_


Preferred (in descending order):

1. Use antsibull-changelog (preferred)
2. Provide ``changelogs/changelog.yaml`` in the `correct format <https://github.com/ansible-community/antsibull-changelog/blob/main/docs/changelog.yaml-format.md>`_
3. Provide a link to the changelog file (self-hosted) (not recommended)

Please note that the porting guide is compiled from ``changelogs/changelog.yaml`` (sections ``breaking_changes``, ``major_changes``, ``deprecated_features``, ``removed_features``). So if you use option 3, you will not be able to add something to the porting guide.

Versioning and deprecation
~~~~~~~~~~~~~~~~~~~~~~~~~~

* Every ansible minor version series (2.10.x) will keep the major version of a collection constant. So if ``community.general`` 1.2.0 is included in 2.10.0, 2.10.x will contain the latest ``community.general`` 1.y.z release that was available when building 2.10.x. If ``community.general`` 2.0.0 is released, it will **not** be included in ansible 2.10.x, but only in 2.11.0 (if it was published before that was released).
* Therefore, please make sure that the current major release of your collection included in 2.10.0 receives at least bugfixes as long new 2.10.x releases are produced.
* Since new minor releases are included, you can include new features, modules and plugins. You must make sure that you do not break backwards compatibility! (See `semantic versioning <https://semver.org/>`_.) This means in particular:
  * You can fix bugs in patch releases, but not add new features or deprecate things.
  * You can add new features and deprecate things in minor releases, but not remove things or change behavior of existing features.
  * Removals and breaking changes can only be made in major releases.
* We recommend to make sure that if a deprecation is added in a collection version that is included in 2.10.x, but not in 2.10.0, that the removal itself will only happen in a collection version included in 2.12.0 or later, but not in a collection version included in 2.11.0.
* Content moved from ansible/ansible that was scheduled for removal in 2.11 or later MUST NOT be removed in the current major release  available when ansible 2.10.0 is released. Otherwise it would already be removed in 2.10, unexpectedly for users! Deprecation cycles can be shortened (since they are now uncoupled from ansible or ansible-base versions), but existing ones must not be unexpectedly terminated.


Repository management
=====================

Repository name
~~~~~~~~~~~~~~~

For collections under ansible-collections the repo SHOULD be named ``NAMESPACE.COLLECTION``

Branch name and configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All new repositories under `ansible-collections<https://github.com/ansible-collections>`_ MUST have `main`` as the default branch.

Existing repositories SHOULD be converted to use ``main``

Repository Protections:
* Allow merge commits: disallowed

Branch protections MUST be enforced
* Require linear history
* Include administrators

CI Testing
===========

At a minimum ``ansible-test sanity`` MUST be run from the `latest stable ansible-base branch <https://github.com/ansible/ansible/branches/all?query=stable->`_. We suggest to *additionally* run ``ansible-test sanity`` from the ansible/ansible ``devel`` branch so that you find out about new linting requirements earlier.

For most repository GitHub actions are sufficient, see `example<https://github.com/ansible-collections/collection_template/tree/main/.github/workflows>`_

FIXME to write a guide "How to write CI tests" (from scratch / add to existing) and put the reference here

Unit Testing
============


Collections and Working Groups
==============================
* Working group page(s) on a corresponding wiki (if needed. Makes sense if there is a group of modules for working with one common entity, e.g. postgresql, zabbix, grafana, etc.)
* Issue for agenda (or pinboard if there aren't regular meetings) as pinned issue in the repository

When moving modules between collections
=======================================

All related entities must be moved / copied including:
* related plugins/module_utils/ files (moving be sure it is not used by other modules, otherwise copy)
* CI and unit tests
* corresponding documentation fragments from plugins/doc_fragments

Also:
* change M(), examples, seealso, extended_documentation_fragments to use actual FQCNs (in moved content and in other collections that have references to the content)
* move all related issues / pull requests / wiki pages


Other things
============
* ansible-base's runtime.yml

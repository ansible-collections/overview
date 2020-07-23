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
* MUST define the minimim version of Ansible which this collection works with
  * If the Collection works with Ansible 2.9, then this should be set to `>=2.9.10`

Modules & Plugins
------------------

Documentation
~~~~~~~~~~~~~~
All module and plugin ``DOCUMENTATION`` and ``RETURN`` MUST:

* Use the FQCN for ``M(...)`` See `Linking within module documentation<https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_documenting.html#linking-within-module-documentation>`_
* Use collection version numbers for `version_added` (otherwise `version_added_collection` must be provided, but this is discouraged)

All module and plugin ``EXAMPLES`` MUST:

* Use FQCN for module (or plugin) name.
* for modules (or plugins) left in ansible-base use ``ansible.builtin.template``

Other items:
* You MUST Use the FQCN for ``extends_documentation_fragment:``, unless you are referring to doc_fragments from ansible-base


Contributor Workflow
====================

Changelogs
~~~~~~~~~~

To give a consistent feel for changelogs across collections, and ensure for collections included in the ``ansible`` package we suggest you use `antsibull-changelog <https://github.com/ansible-community/antsibull-changelog>`_


Preferred (in descending order):

1. Use antsibull-changelog (preffered)
2. Provide ``changelogs/changelog.yaml`` in the `correct format <https://github.com/ansible-community/antsibull-changelog/blob/main/docs/changelog.yaml-format.md>`_
3. Provide a link to the changelog file (self-hosted) (not recommended)

Please note that the porting guide is complied from `changelogs/changelog.yaml` (sections `breaking_changes`, `major_changes`, `deprecated_features`, `removed_features`). So if you use option 3, you will not be able to add something to the porting guide.

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

At a minimim ``ansible-test sanity`` MUST be run from the `latest stable ansible-base branch <https://github.com/ansible/ansible/branches/all?query=stable->`_. We suggest to *additionally* run ``ansible-test sanity`` from the ``devel`` branch so that you find out about new linting requirements earlier.

For most repos GitHub actions are sufficient, see `example<https://github.com/ansible-collections/collection_template/tree/main/.github/workflows>`_

FIXME to write a guide "How to write CI tests" (from scratch / add to existing) and put the reference here

Unit Testing
============


Collections and Working Groups
==============================
* Working group page(s) on a corresponding wiki (if needed. Makes sense if there is a group of modules for working with one common entity, e.g. postgresql, zabbix, grafana, etc.)
* Issue for agenda (or pinboard if there aren't regular meetings) as pinned issue in the Repo

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

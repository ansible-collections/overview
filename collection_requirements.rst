*****************************
Ansible Collections Checklist
*****************************

.. contents:: Topics

Overview
========
This document is for maintainers of collections to provide them help, advice, and guidance on making sure their collections are correct.

**Warning:** Subject to frequent updates. This is a "living document", expect it to change as we progress with the Collections work over the next few months.

**Note:** Inclusion of a new collection in the Ansible package is ultimately at the discretion of the Ansible community review committee. Every rejected candidate will get feedback. Differences of opinion should be taken to the community irc meeting for discussion and a final vote.


Feedback
========

As with any project it's very important that we get feedback from users, contributors and maintainers. We recognize that the move to Collections is a big change in how Ansible is developed and delivered.

Please raise feedback by:

* Discussing in ``#ansible-community`` in Freenode IRC
* Adding to the `Community Working Group IRC meeting <https://github.com/ansible/community/issues/539>`_
* Creating `GitHub Issues <https://github.com/ansible-collections/overview/issues>`_ against this repository

Keeping informed
================

Be sure you're subscribed to:

* `Changes impacting Collections <https://github.com/ansible-collections/overview/issues/45>`_ to track changes that Collection maintainers should be aware of
* The Bullhorn, a newsletter for the Ansible developer community, `back issues and how to add content <https://github.com/ansible/community/issues/546>`_

Why is this needed
===================

Collection Infrastructure
=========================

* MUST have a publicly available issue tracker, that does not require a paid level of service to create an account or view issues.
* Collections MUST have a Code of Conduct (CoC)

  * The collection's CoC MUST be compatible with the Ansible CoC
  * Collections SHOULD consider using the Ansible CoC if they do not have a CoC that they consider better
  * The Diversity and Inclusion working group may evaluate all CoCs and object to a collection's inclusion based on the CoCs contents
  * The CoC must be linked from the README.md file, or must be present or linked from a CODE_OF_CONDUCT.md file in the collection root.
  
* MUST be published to `Ansible Galaxy <https://galaxy.ansible.com>`_.

Repo structure
===============

galaxy.yml
----------

* The ``tags`` field MUST be set
* Collection dependencies must have a lower bound on the version which is at least 1.0.0.

  * This means that all collection dependencies have to specify lower bounds on the versions, and these lower bounds should be stable releases, and not versions of the form 0.x.y.
  * When creating new collections where collection dependencies are also under development, you need to watch out since Galaxy checks whether dependencies exist in the required versions:

    1. Assume that ``foo.bar`` depends on ``foo.baz``
    2. First release ``foo.baz`` as 1.0.0.
    3. Then modify ``foo.bar``'s ``galaxy.yml`` to specify ``'>=1.0.0'`` for ``foo.baz``
    4. Finally release ``foo.bar`` as 1.0.0

* The ``ansible`` package MUST NOT depend on collections not shipped in the package.
* If you plan to split up your collection, the new collection must be approved for inclusion before the smaller collections replace the larger in Ansible.
* If you plan to add other collections as dependencies, they must run through the formal application process.

README.md
---------

MUST have a ``README.md`` in the root of the Collection, see `collection_template/README.md <https://github.com/ansible-collections/collection_template/blob/main/README.md>`_ for an example.

meta/runtime.yml
----------------
Example: `meta/runtime.yml <https://github.com/ansible-collections/collection_template/blob/main/meta/runtime.yml>`_

* MUST define the minimum version of Ansible which this collection works with

  * If the collection works with Ansible 2.9, then this should be set to `>=2.9.10`
  * It's usually better to avoid adding `<2.11` as a restriction, since this for example makes it impossible to use the collection with the current ansible-base devel branch (which has version 2.11.0.dev0)

Modules & Plugins
------------------

* Collections MUST only put plugin types recognized by ansible-core into the :file:`plugins/` directory at this time.  The recognized plugin types are listed on https://docs.ansible.com/ansible/devel/plugins/plugins.html plus ``terminal``, ``modules``, and ``module_utils``.

  * The following collections have a temporary exception to use the ``plugin_utils``, ``cli_parsers``, ``fact_diff``, and ``validate`` directories for additional plugins during the 2.10 and 3.0 release cycles.  We will figure out a final policy which these collections will need to comply with before ansible-4.0:

    * ansible.utils
    * ansible.netcommon
    * community.crypto
    * community.docker
    * community.sops

Documentation
~~~~~~~~~~~~~~
All module and plugin ``DOCUMENTATION`` and ``RETURN`` MUST:

* Use the FQCN for ``M(...)`` and ``- module:`` references of ``seealso`` subsections. See `Linking within module documentation <https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_documenting.html#linking-within-module-documentation>`_
* Use the field ``version_added`` to document the version of the collection for which an option, module or plugin was added.

  * Use collection version numbers for ``version_added``, and not Ansible version numbers or other unrelated version numbers.
  * If you for some reason really have to specify version numbers of Ansible or of another collection, you have to provide ``version_added_collection``. We strongly recommend to NOT do this.
  * Not every option, module or plugin must have ``version_added``. You should use it to mark when new content (modules, plugins, options) were added to the collection. The values are shown in the documentation, and this can be very useful for your users.

All module and plugin ``EXAMPLES`` MUST:

* Use FQCN for module (or plugin) name.
* For modules (or plugins) left in ansible-base use ``ansible.builtin.`` as a FQCN prefix, for example, ``ansible.builtin.template``

Other items:

* You MUST Use the FQCN for ``extends_documentation_fragment:``, unless you are referring to doc_fragments from ansible-base
* The ``CONTRIBUTING.md`` (or ``README.md``) file MUST state what types of contributions (pull requests, feature requests, and so on) are accepted and any relevant contributor guidance. Issues (bugs and feature request) reports must always be accepted

Contributor Workflow
====================

Changelogs
----------

Collections are required to include a changelog.  To give a consistent feel for changelogs across collections and ensure changelogs exist for collections included in the ``ansible`` package we suggest you use `antsibull-changelog <https://github.com/ansible-community/antsibull-changelog>`_ to maintain and generate this but other options exist.  Preferred (in descending order):


1. Use antsibull-changelog (preferred)
2. Provide ``changelogs/changelog.yaml`` in the `correct format <https://github.com/ansible-community/antsibull-changelog/blob/main/docs/changelog.yaml-format.md>`_
3. Provide a link to the changelog file (self-hosted) (not recommended)

Please note that the porting guide is compiled from ``changelogs/changelog.yaml`` (sections ``breaking_changes``, ``major_changes``, ``deprecated_features``, ``removed_features``). So if you use option 3, you will not be able to add something to the porting guide.

Versioning and deprecation
~~~~~~~~~~~~~~~~~~~~~~~~~~

* Collections MUST adhere to `semantic versioning <https://semver.org/>`_.
* To preserve backward compatibility for users, every ansible minor version series (2.10.x) will keep the major version of a collection constant. If ansible 2.10.0 includes ``community.general`` 1.2.0, then each 2.10.x release will include the latest ``community.general`` 1.y.z release available at build time. Ansible 2.10.x will **never** include a ``community.general`` 2.y.x release, even if it is available. Major collection version changes will be included in the next ansible minor release (2.11.0, 2.12.0, and so on).
* Therefore, please make sure that the current major release of your collection included in 2.10.0 receives at least bugfixes as long new 2.10.x releases are produced.
* Since new minor releases are included, you can include new features, modules and plugins. You must make sure that you do not break backwards compatibility! (See `semantic versioning <https://semver.org/>`_.) This means in particular:

  * You can fix bugs in patch releases, but not add new features or deprecate things.
  * You can add new features and deprecate things in minor releases, but not remove things or change behavior of existing features.
  * You can only remove things or make breaking changes in major releases.
* We recommend to make sure that if a deprecation is added in a collection version that is included in 2.10.x, but not in 2.10.0, that the removal itself will only happen in a collection version included in 2.12.0 or later, but not in a collection version included in 2.11.0.
* Content moved from ansible/ansible that was scheduled for removal in 2.11 or later MUST NOT be removed in the current major release  available when ansible 2.10.0 is released. Otherwise it would already be removed in 2.10, unexpectedly for users! Deprecation cycles can be shortened (since they are now uncoupled from ansible or ansible-base versions), but existing ones must not be unexpectedly terminated.
* We recommend to announce your policy of releasing, versioning and deprecation to contributors and users in some way. For an example of how to do this, see `the announcement in community.general <https://github.com/ansible-collections/community.general/issues/582>`_. You could also do this in the README.


Naming
======

For collections under ansible-collections the repository SHOULD be named ``NAMESPACE.COLLECTION``.

To create a new collection and corresponding repository, first, a new namespace in Galaxy has to be created via submitting `Request a namespace <https://github.com/ansible/galaxy/issues/new/choose>`_.

`Namespace limitations <https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations>`_  lists requirements for namespaces in Galaxy.

For collections created for working with a particular entity, they should contain the entity name, for example ``community.mysql``.

For corporate maintained collections, the repository can be named ``COMPANY_NAME.PRODUCT_NAME``, for example ``ibm.db2``.

We should avoid FQCN / repository names:

* which are unnecessary long: try to make it compact but clear
* contain the same words / collocations in ``NAMESPACE`` and ``COLLECTION`` parts, for example ``my_system.my_system``


Licensing
=========

At the moment, ``module_utils`` must be licensed under the `BSD-2-clause <https://opensource.org/licenses/BSD-2-Clause>`_ or `GPL-3.0-or-later <https://www.gnu.org/licenses/gpl-3.0-standalone.html>`_ license and all other content must be licensed under the `GPL-3.0-or-later <https://www.gnu.org/licenses/gpl-3.0-standalone.html>`_.  We will have a list of other open source licenses which are allowed as soon as we get Red Hat's legal team to approve such a list for us.


Repository management
=====================

Branch name and configuration
-----------------------------

This subsection is **only** for repositories under `ansible-collections <https://github.com/ansible-collections>`_! Other collection repositories can also follow these guidelines, but do not have to.

All new repositories MUST have ``main`` as the default branch.

Existing repositories SHOULD be converted to use ``main``

Repository Protections:

* Allow merge commits: disallowed

Branch protections MUST be enforced:

* Require linear history
* Include administrators

CI Testing
===========

* You MUST run ``ansible-test sanity`` from the `latest stable ansible-base/ansible-core branch <https://github.com/ansible/ansible/branches/all?query=stable->`_. 

  * Collections must run an equivalent of ``ansible-test sanity --docker``. If they do not use ``--docker``, they must make sure that all tests run, in particular the compile and import tests (which should run for all Python versions). Collections can choose to skip certain Python versions that they explicitly do not support; this needs to be documented in ``README.md`` and in every module and plugin (hint: use a docs fragment).

* You SHOULD suggest to *additionally* run ``ansible-test sanity`` from the ansible/ansible ``devel`` branch so that you find out about new linting requirements earlier.
* The sanity tests MUST pass.

  * Adding some entries to the ``test/sanity/ignore*.txt`` file is an allowed method of getting them to pass, except cases listed below.
  * You SHOULD not have ignored test entries.  A reviewer can manually evaluate and approve your collection if they deem an ignored entry to be valid.

  * You MUST not ignore the following validations. They must be fixed before approval:
      * ``validate-modules:doc-choices-do-not-match-spec``
      * ``validate-modules:doc-default-does-not-match-spec``
      * ``validate-modules:doc-missing-type``
      * ``validate-modules:doc-required-mismatch``
      * ``validate-modules:mutually_exclusive-unknown``
      * ``validate-modules:nonexistent-parameter-documented``
      * ``validate-modules:parameter-list-no-elements``
      * ``validate-modules:parameter-type-not-in-doc``
      * ``validate-modules:undocumented-parameter``

  * All entries in ignores.txt MUST have a justification in a comment in the ignore.txt file for each entry.  For example ``plugins/modules/docker_container.py use-argspec-type-path # uses colon-separated paths, can't use type=path``.
  * Reviewers can block acceptance of a new collection if they don't agree with the ignores.txt entries.

* You MUST run CI against each of the "major versions" (2.10, 2.11, 2.12, etc) of ``ansible-base``/``ansible-core`` that the collection supports. (Usually the ``HEAD`` of the stable-xxx branches.)

* All CI tests MUST run against every pull request and SHOULD pass before merge.
* All CI tests MUST pass for the commit that releases the collection.
 
* All CI tests MUST run regularly (nightly, or at least once per week) to ensure that repos without regular commits are tested against the latest version of ansible-test from each ansible-base/ansible-core version tested. 

All of the above can be achieved by using the following GitHub Action template, see this `example <https://github.com/ansible-collections/collection_template/tree/main/.github/workflows>`_.


FIXME to write a guide "How to write CI tests" (from scratch / add to existing) and put the reference here.

Unit Testing
============


Collections and Working Groups
==============================

* Working group page(s) on a corresponding wiki (if needed. Makes sense if there is a group of modules for working with one common entity, for example postgresql, zabbix, grafana, and so on.)
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
* look through ``docs/docsite`` directory of `ansible-base GitHub repository <https://github.com/ansible/ansible>`_ (for example, using the ``grep`` command-line utility) to check if there are examples using the moved modules / plugins to update their FQCNs

See `Migrating content to a different collection <https://docs.ansible.com/ansible/devel/dev_guide/developing_collections.html#migrating-ansible-content-to-a-different-collection>`_ for complete details.


Requirements for collections to be included in the Ansible Package
==================================================================

To be included in the `ansible` package, collections must meet the following criteria:

* `development conventions <https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_best_practices.html>`_
* `Collection requirements <https://github.com/ansible-collections/overview/blob/main/collection_requirements.rst>`_ (this document)
* `Ansible documentation format <https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_documenting.html>`_ and the `style guide <https://docs.ansible.com/ansible/devel/dev_guide/style_guide/index.html#style-guide>`_
* to pass the Ansible `sanity tests <https://docs.ansible.com/ansible/devel/dev_guide/testing_sanity.html#testing-sanity>`_
* to have `unit <https://docs.ansible.com/ansible/devel/dev_guide/testing_units.html#unit-tests>`_ and / or `integration tests <https://docs.ansible.com/ansible/devel/dev_guide/testing_integration.html#integration-tests>`_ according to the corresponding sections of this document


Other things
============

* ansible-base's runtime.yml
* After content is (moved out of community.general or community.network) OR new collection satisfies all the requirements
    * Add the collection to the ``ansible.in`` file in a corresponding directory of `ansible-build-data repository <https://github.com/ansible-community/ansible-build-data/>`_

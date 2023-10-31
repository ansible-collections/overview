****************************
Ansible Collections Overview
****************************

.. contents:: Topics

Overview
========

**Warning:** Subject to frequent updates
       This is a "living document", expect it to change as we progress with the Collections work over the next few months.

Feedback
========

As with any project it's very important that we get feedback from users, contributors and maintainers. We recognize that the move to Collections is a big change in how Ansible is developed and delivered. Therefore you should expect that **the devel branch may be broken** on occasion.

See the `Ansible Communication channels <https://docs.ansible.com/ansible/latest/community/communication.html>`_ for a list of IRC channels and email lists you can use to join the discussion.

* Discussing in the ``#ansible-community`` `libera.chat <https://libera.chat/>`_ IRC channel.
* Adding to the `Community Working Group IRC meeting <https://github.com/ansible/community/issues/539>`_.
* Creating `GitHub Issues <https://github.com/ansible-collections/overview/issues>`_ against this repo.

Where we've come from, and where we are going
=============================================

To understand where we are going, it's often useful to remind ourselves of where we came from.

Previously: Ansible 2.9 and earlier
-----------------------------------

**Classic Ansible**

* Single repository `ansible/ansible <https://github.com/ansible/ansible>`_
* Single package called `ansible`
* `ansible` had major releases twice a year
* New features go into the next major release — i.e., in the worst case, you need to wait 6 months.


Now: Ansible 2.10 and later
---------------------------

* The ``ansible/ansible`` (`ansible-base`) repository only contains:

  * The core Ansible programs, ``ansible-{playbook,galaxy,doc,test,etc}``
  * Some documentation
  * A tiny subset of modules and plugins to allow for a functioning controller
  * Together this will be known as ``ansible-base``.
* The rest of the modules and plugins have been moved into various "collections"

  * Ansible Collections:

    * Can be released independently of ansible-base and Ansible, at whatever release cycle/cadence the collection maintainer prefers.
    * Will have their own repo (GitHub, GitLab, etc) with dedicated backlog, i.e. no more shared massive issue and PR backlog
    * Should still have CI testing and in many cases can be tested more thoroughly

* The released package of Ansible 2.10 will pull in ``ansible-base`` and the various community collections that were previously a part of ``ansible/ansible``

The ``ansible`` package (will contain a subset of collections) and depend on the new ``ansible-base`` package (the Ansible engine).

Terminology
===========


Classic Ansible (CA)
  What we think of as Ansible prior to 2020. The open source project and the deliverable. What you currently get (as of Ansible 2.9 and earlier versions) when you type ``pip install ansible``. Was developed almost entirely within the ``ansible/ansible`` repository on GitHub.

Collection
  A packaging format for bundling and distributing Ansible content: plugins, roles, modules. Can be released independent of other collections or ``ansible-base`` so features can be made available sooner to users. Installed via ``ansible-galaxy collection install <namespace.collection>``.


``ansible`` (the package)
  A replacement software package (Python, deb, rpm, etc) which contains a select group of Collections. It contains the collections to ensure that Ansible 2.9 playbooks don't require any extra collections to be be installed. This list of what's included can be found at `ansible-build-data <https://github.com/ansible-community/ansible-build-data/tree/master/2.10>`_. Will depend on the ``ansible-base`` package. Was previously referred to as ``ACD`` (Ansible Community Distribution).

``ansible-base``
  New for 2.10. The codebase that is now contained in github.com/ansible/ansible for the Ansible 2.10 release. It contains a minimal amount of modules and plugins and allows other collections to be installed. Similar to Ansible 2.9 though without any content that has since moved into a collection. The devel branch of ``ansible/ansible`` is now ansible-base.

There will be an ``ansible-base`` package (RPM/Python/Deb package)with only the minimal set of modules and plugins).

Ansible Galaxy
  An online hub for finding and sharing Ansible community content.  Also, the command-line utility that lets users install individual Ansible Collections, ``ansible-galaxy install community.crypto``. `galaxy.ansible.com <https://galaxy.ansible.com/>`_.

Fully Qualified Collection Name (FQCN)
  The full definition of a module, plugin, or role hosted within a collection, in the form ``namespace.collection.content_name``. Allows a Playbook to refer to a specific module or plugin from a specific source in an unambiguous manner, for example, ``community.grafana.grafana_dashboard``. The FQCN is required when you want to specify the exact source of a module and multiple modules with the same name are available. Can always be identified in a playbook; ideally not necessary in most playbooks, but in cases in which users have multiple collections installed with similar content, the FQCN will always be the explicit and authoritative indicator of which collection to use for content. Example: ``cisco.ios.ios_config`` would be the FQCN, and the playbook would generally call "ios_config" when this is required.

Namespace
  The first part of a Fully Qualified Collection Name, the namespace usually reflects a functional content category. Example: in ``cisco.ios.ios_config``, “Cisco” is the Namespace. Namespaces are reserved and distributed by Red Hat at Red Hat’s discretion. Many, but not all, namespaces will correspond with vendor names.

Collection name
  In the second part of a Fully Qualified Collection Name, the collection name further divides the functional characteristics of the collection content and denotes ownership.  For example, the cisco namespace might contain  ``cisco.ios``, ``cisco.ios_community``, and ``cisco.ios_prc``, containing content for managing ios network devices maintained by Cisco.

community.general (collection)
  A special collection managed by the Ansible Community Team containing all the modules and plugins which shipped in Ansible 2.9 that don't have their own dedicated Collection. See community.general on `Galaxy <https://galaxy.ansible.com/community/general>`_ or it's `GitHub repository <https://github.com/ansible-collections/community.general/>`_ .

community.network (collection)
  Similar to ``community.general``, though focusing on Network modules. See community.network on `Galaxy <https://galaxy.ansible.com/community/network>`_ or it's `GitHub repository <https://github.com/ansible-collections/community.network/>`_ .

Repository
  The location of the source code included in a collection. Contributors make suggestions, fix bugs, and add features through the repository. Collection owners can host repositories on GitHub, Gerrit, or any other source code repository platform they choose.

Although this document focuses on Community (upstream) content, there will be Product (downstream) equivalents of the above. Links to the Product documentation will be added once they are available.

Documentation
==============

* `Using Ansible Collections <https://docs.ansible.com/ansible/latest/user_guide/collections_using.html>`_
* `Developing Collections <https://docs.ansible.com/ansible/latest/dev_guide/developing_collections.html>`_
* `Ansible Collections Requirements <https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html>`_

Work needed
===========

Ansible 2.9 already contains basic support for Collections.

The majority of the ansible-base 2.10 release cycle is for:

* Defining what the split of collections should be
* Defining which modules and plugins go into these new collections
* Defining ansible-base (ie which modules stay in ansible/ansible)
* Updating test infrastructure
* Testing the changes
* Getting feedback from *you*


Timeline
--------

* See `status.rst <https://github.com/ansible-collections/overview/blob/master/status.rst>`_ for dates of  beta, RC, Release dates for ``ansible 2.10``
* See `ROADMAP_2.10 <https://github.com/ansible/ansible/blob/devel/docs/docsite/rst/roadmap/ROADMAP_2_10.rst>`_ for dates of  beta, RC, Release dates for ``ansible-base 2.10``

FAQ
====

Users of Ansible
-----------------

`Using Ansible Collections <https://docs.ansible.com/ansible/latest/user_guide/collections_using.html>`_

Q: Once the next version of Ansible is released, will my playbooks still work?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For users of the community version of ansible ``pip/apt-get install ansible`` will continue to give you a working install of Ansible including the three thousand plus modules that previously shipped with Ansible 2.9.

Q: I deploy Ansible directly from devel. Is that still advised?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We recognize that many users install Ansible directly from devel, and generally we do our best to keep the devel branch stable. These changes, however, will have a very large short-term impact, and we cannot guarantee that devel will be as stable as it has been in the past through this migration process.

These changes, however, will have both a temporary and permanent impact.

* Temporarily

  * These changes are large and invasive so there may be bugs which break many things.
  * We cannot guarantee that devel will be as stable as it has been in the past during this transition period.
* Permanent

  * Users of devel will need to get both ansible-base (the package with contains ``ansible-playbook``) and the ansible collections that their playbooks rely on. The collections will reside in multiple other git repositories (or can be installed from galaxy).
  * If your workflow presently updates your checkout of the ansible devel branch, you'll need to change it to also retrieve the collections you need otherwise your playbooks will fail,

Q: I'd like to consume the development stream of Ansible during this process. How do I do that?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can pip install ansible-base by doing:

``python -m pip install --user https://github.com/ansible/ansible/archive/devel.tar.gz``

Individual collections can be installed by doing:

``ansible-galaxy collection install NAMESPACE.COLLECTION``

Q: What exactly is ansible-base for and what does it contain?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Ansible-base** is the name of the code and package for what github.com/ansible/ansible has become now that most of the content has been removed.

use-cases for ansible-base
""""""""""""""""""""""""""

* ``ansible[|-playbook|-galaxy|-pull|-doc|-test]`` --help
* Being able to install content from Galaxy or Automation Hub

  * ``ansible-galaxy collection ...``
  * Setup Networking
  * Setup Proxy
* Being able to install supported content via packages

  * ie RHEL users will not use ``ansible-galaxy collection install ...``, they want RPMs
  * Ability to setup and use package repos
  * Ability to work online or offline

* Include things that are "hardcoded" into Ansible

  * eg ``stat`` is used to handle any file information internally
  * ``include_tasks`` is hardcoded as the implementation is inside the engine, same with ``add_host``, ``group_by``, ``debug`` and others, async_wrapp, async-poll, assert/fail are 'parts of the language'
* Development

  * Ability to run ``ansible-test sanity,units,integration`` against the Ansible code base
* Parts of the Windows codebase that can't currently be removed from ansible-base.

Bugs in ansible-base should be reported via  `ansible/ansible issues <https://github.com/ansible/ansible/issues/new/choose>`_.

Contributors to Ansible
------------------------

`Developing Collections <https://docs.ansible.com/ansible/latest/dev_guide/developing_collections.html>`_

`Ansible Collections Requirements <https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html>`_

Q: I'd like to submit a new module or plugin to Ansible. How shall I proceed?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you're a vendor/partner and you're writing Ansible content to interact with your software, we recommend writing your own collection. This will allow you to pursue certification against the Ansible Automation Platform. For more info on certification, read the `Partners Page <http://ansible.com/partners>`_.

If you want to submit your module to an existing collection, you'll want to coordinate with the maintainers of those collections and follow their guidelines.

As of today **ansible-core (github.com/ansible/ansible) will no longer accept new modules or plugins.**

Q: I'd like to submit a pull request to an existing Ansible module. How will I know where this module will live?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We will have a `mapping <https://docs.ansible.com/ansible/devel/dev_guide/developing_collections_migrating.html>`_ of old modules to their new homes. Should you submit a PR to the wrong repository, we will close it and point you to the correct repository.

Q: I have existing pull requests to existing modules that will become part of the Ansible Community Collection. What will happen to those?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Pull requests not merged before the freeze, will need to be recreated in the corresponding new Collection Repo. We will have a tool to help move PRs from one repo to another.

Maintainers of Ansible content
------------------------------

Q: What happens if I don't move my content into a collection?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Content that doesn't end up in its own Collection will end up being automatically migrated to ``community.general`` during the devel freeze window.

Q: Why would I want a dedicated collection?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The benefits of claiming content are the following:

* Source content is housed in a GitHub organization/repository of your choosing
* Source content is subject to your own CI processes, decisions, and testing
* Your own dedicated Issue and PR backlog
* Ability to use more GitHub functionality, such as direct assignments, reviews, milestones and Project Boards

Q: What will versioning and deprecation look like for Collections?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* In ansible/ansible:

  * There is a single version number which is over everything shipped in Ansible
  * Doesn't use semver, uses X.Y (ie 2.9) as the major number
  * Deprecations are done over 4 versions (~ 2 years)
* In Collections

  * Can be versioned and released independently to Ansible
  * By convention, Galaxy requires a Collection to follow `semver (Semantic Versioning) <https://semver.org/>`_

Details around versioning and deprecation policy are still being worked on, we will have a proposal up shortly


Q: What should I do to move plugins across collections during migration?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

See `Migrating content to a different collection <https://docs.ansible.com/ansible/devel/dev_guide/developing_collections.html#migrating-ansible-content-to-a-different-collection>`_ .


Q: How can I fix bugs in Ansible 2.9?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Please note that Ansible 2.9 no longer receives bugfixes. Only security issues can be fixed, and eventually it will be end of life with no more fixes accepted. See `ansible-core release cycle <https://docs.ansible.com/ansible/devel/reference_appendices/release_and_maintenance.html#ansible-core-release-cycle>`_ for whether 2.9 is still accepting security fixes or not.

The process for fixing a security issue is as follows:

1. PR for bug fix made against the Collection
2. PR gets merged into Collection
3. Raise PR directly against ``ansible/ansible:stable-2.9`` (ie not a backport) including a ``changelogs/fragments/`` file

The changes in the PR against ``ansible/ansible:stable-2.9`` should be as close as possible to the changes in the collection original PR, and you should add a reference to the collection PR in the ``ansible/ansible`` PR.

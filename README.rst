.. contents:: Topics

Overview
========

**Warning:** Subject to frequent updates
       This is a "living document", expect it to change as we progress with the Collections work over the next few months.

Feedback
========

As with any project it's very important that we get feedback from users, contributors and maintainers. We recognise that the move to Collections is a big change in how Ansible is developed and delivered. Therefore you should expect that **the devel branch maybe broken** on occasion.

See the `Ansible Communication channels <https://docs.ansible.com/ansible/latest/community/communication.html>`_ for a list of IRC channels and email lists you can use to join the discussion. For specific issues please `raise a bug report <https://github.com/ansible/ansible/issues/new/choose>`_.

Where we've come from, and where we are going
=============================================

To understand where we are going, it's often useful to remind ourselves of where are today.

Ansible 2.9 and earlier
------------------------

**Classic Ansible**

* Single repository `ansible/ansible <https://github.com/ansible/ansible>`_.
* Single packages called `ansible`
* `ansible` has major releases twice a year
* New features go into the next major release - ie worst case you need to wait 6 months


Ansible 2.10 and later
----------------------

* The ``ansible/ansible`` repository will only contain:

  * The core Ansible programs, ansible-(playbook,galaxy,docs,test,etc)
  * Documentation
  * A tiny subset of modules and plugins to allow for a functioning controller
  * Together this will be known as ``ansible-base``
* The rest of the modules and plugins will move into multiple Collections
* Collections:

  * Can be released independently of Ansible, at whatever release cycle/cadence the collection maintainer prefers.
  * Will have their own repo (GitHub, GitLab, etc) with dedicated backlog, ie no more shared massive issue & PR backlog
  * Should still have CI testing and in many cases can be tested more thoroughly

* The released package of Ansible 2.10 will pull in ``ansible-base`` and the various community collections that were previously a part of ``ansible/ansible``


Terminology
===========


Classic Ansible (CA)
  What we think of as Ansible prior to 2020. The open source project and the deliverable. What you currently get (as of Ansible 2.9) when you type ``pip install ansible``. Currently developed almost entirely within the ``ansible/ansible`` repository on GitHub.

Collection
  A packaging format for bundling and distributing Ansible content: plugins, roles, modules. Can be released independent of other collections or ``ansible-base`` so features can be made available sooner to users. Installed via ``ansible-galaxy collection install <namespace.collection>``.

Ansible Base
  The codebase that will be contained in github.com/ansible/ansible for the Ansible 2.10 release. Contains a minimal amount of modules and plugin, allows other collections to be installed. Similar to Ansible 2.9 though without any content that has since moved into a collection. A work in progress can be found in `ansible-base <https://github.com/ansible-collection-migration/ansible-base/>`_ repository.

At this time it is unknown if there will be an ``ansible-base`` package (ie a RPM/Python/Deb package with only the minimal set of modules and plugins).

Ansible Community Content Collections
  The collections which host the modules and plugins, previously in Ansible 2.9, which are managed by the community. These collections will be combined and released when you do ``$package_manager install ansible``. These may run at different cadences, and may now be installed individually by users via Ansible Galaxy.

Ansible Galaxy
  An online hub for finding and sharing Ansible community content.  Also, the command-line utility that lets users  install individual Ansible Collections. `galaxy.ansible.com <https://galaxy.ansible.com/>`_.

Fully Qualified Collection Name (FQCN)
  The full definition of a module, plugin, or role hosted within a collection, in the form ``namespace.collection.content_name``. Allows a Playbook to refer to a specific module or plugin from a specific source in an unambiguous manner, for example, ``community.grafana.grafana_dashboard``. The FQCN is required when you want to specify the exact source of a module and multiple modules with the same name are available. Can always be identified in a playbook; ideally not necessary in most playbooks, but in cases in which users have multiple collections installed with similar content, the FQCN will always be the explicit and authoritative indicator of which collection to use for content. Example: cisco.ios.ios_config would be the FQCN, and the playbook would generally call "ios_config". When is this required.

Namespace
  The first part of a Fully Qualified Collection Name, the namespace usually reflects a functional content category. Example: in ``cisco.ios.ios_config``, “Cisco” is the Namespace. Namespaces are reserved and distributed by Red Hat at Red Hat’s discretion. Many, but not all, namespaces will correspond with vendor names.

Collection name
  In the second part of a Fully Qualified Collection Name, the collection name further divides the functional characteristics of the collection content and denotes ownership.  For example, the cisco namespace might contain  ``cisco.ios``, ``cisco.ios_community``, and ``cisco.ios_prc``, containing content for managing ios network devices maintained by Cisco.

The community.general collection
  A special collection managed by the Ansible Community Team containing all the modules and plugins which shipped in Ansible 2.9 that don't have their own dedicated Collection. A work in progress can be found in `community.general <https://github.com/ansible-collection-migration/community.general/>`_ repository. At least initially there are no Long Term Support (LTS) plans, though we will see how the need for that grows over time.

Repository
  The location of the source code included in a collection. Contributors make suggestions, fix bugs, and add features through the repository. Collection owners can host repositories on GitHub, Gerrit, or any other source code repository platform they choose.

Although this document focuses on Community (upstream) content, there will be Product (downstream) equivalents of the above. Links to the Product documentation will be added once they are available.

Documentation
==============

* `Using Ansible Collections <https://docs.ansible.com/ansible/latest/user_guide/collections_using.html>`_
* `Developing Collections <https://docs.ansible.com/ansible/latest/dev_guide/developing_collections.html>`_

Work needed
===========

Ansible 2.9 already contains basic support for Collections.

The majority of the Ansible 2.10 release cycle is for:

* Defining what the split of collections should be
* Defining which modules and plugins go into these new collections
* Defining ansible-base (ie which modules stay in ansible/ansible)
* Updating test infrastructure
* Testing the changes
* Getting feedback from *you*

We will soon begin the migration of content out of ansible/ansible, into its new component collection repositories.

Timeline
--------

**Warning:** Dates subject to change

* **2nd March 2020**, we will freeze the devel branch using protected branches, and we will create the temp-2.10-devel branch from devel. This date marks the end of merging non-base plugin/module PRs into ansible/ansible.

* **9th March 2020**, we will perform the initial migration against temp-2.10-devel, and we will do our initial testing of the components.

* **23rd March 2020**, we intend to unfreeze devel and merge temp-2.10-devel back into devel. From that point on, devel for ansible/ansible will be for the ansible-base project only.

* TBC, ``community.general`` accepts new Pull Requests (PRs).

* TBC, The ``ansible`` package has been updated to include the Community Collections.

* TBC alpha, beta, RC, Release dates for Ansible 2.10

FAQ
====

Users of Ansible
-----------------

`Using Ansible Collections <https://docs.ansible.com/ansible/latest/user_guide/collections_using.html>`_

Q: Once the next version of Ansible is released, will my playbooks still work
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For users of the community version of ansible pip/apt-get install ansible will continue to give you a working install of Ansible including the three thousand plus modules.

Q: I deploy Ansible directly from devel. Is that still advised?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We recognize that many users install Ansible directly from devel, and generally we do our best to keep the devel branch stable. These changes, however, will have a very large short-term impact, and we cannot guarantee that devel will be as stable as it has been in the past through this migration process.

Q: When will the next version of Ansible be released with these changes?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We don't have a firm date yet, but we plan to release Ansible 2.10 sometime in 2020, and we do expect to have several alpha/beta releases between now and then. Until that time, Ansible 2.9 will continue to be the supported version.

Q: I'd like to consume the development stream of Ansible during this process. How do I do that?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once the split has been successfully accomplished, devel users will consume ansible-base and collections separately. We will provide instructions for installing the various components from devel when it is unfrozen on March 23.

Q: What exactly is ansible-base for and what will it contain
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Ansible-base** is the name for what github.com/ansible/ansible will become once content has been removed and ``temp-2.10-devel`` has been merged back into ``devel`` branch.

use-cases for ansible-base
""""""""""""""""""""""""""

 ``ansible[|-playbook|-galaxy|-pull|-doc|-test]`` --help
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
  * ``include_tasks`` is hardcoded as the implementation is inside the engine, same with ``add_hosts``, ``group-by``, ``debug`` and others, async_wrapp, async-poll, assert/fail are 'parts of the language'
* Development
  * Ability to run ``ansible-test sanity,unit,integration`` against the Ansible code base
* Parts of the Windows codebase that can't currently be removed from ansible-base.

pre-release versions of ansible-base
""""""""""""""""""""""""""""""""""""

If you wish to look at the current state of ansible-base you can:
* See the `scenario <https://github.com/ansible-collection-migration/ansible-base>`_ which defines what goes into ansible-base
* Checkout and run the source https://github.com/ansible-collection-migration/ansible-base (pip install in a Python virtual environment, or do ``source hacking/env-setup``)

If you spot an problems with ansible-base between now and ``temp-2.10-devel`` has been merged back into ``devel`` branch please raise them via `collection_migration <https://github.com/ansible-community/collection_migration/issues/new/>`_, after this point use `ansible/ansible issues <https://github.com/ansible/ansible/issues/new/choose>`_

Contributors to Ansible
------------------------

`Using Developing Collections <https://docs.ansible.com/ansible/latest/dev_guide/developing_collections.html>`_

Q: I'd like to submit a new module or plugin to Ansible. How shall I proceed?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you're a vendor/partner and you're writing Ansible content to interact with your software, we recommend writing your own collection. This will allow you to pursue certification against the Ansible Automation Platform. For more info on certification, read here [FIXME: link].

If you want to submit your module to the ``community.general`` Collection, please wait till this repo has been created (see timeline at the top of this document).

If you want to submit your module to an existing collection, you'll want to coordinate with the maintainers of those collections and follow their guidelines. Note that not all collections will necessarily accept new modules, nor follow the guidelines that ansible/ansible previously did.

As of today **Ansible Base (and ansible/ansible) will no longer accept new modules.**

Q: I'd like to submit a pull request to an existing Ansible module. How will I know where this module will live?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We will have a `mapping <https://docs.ansible.com/ansible/devel/dev_guide/developing_collections.html#migrating-ansible-content-to-a-collection>`_ of old modules to their new homes. Should you submit a PR to the wrong repository, we will close it and point you to the correct repository.

For new PRs please wait for the new Collections to be created.

Q: I have existing pull requests to existing modules that will become part of the Ansible Community Collection. What will happen to those?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Pull requests merged before ``ansible/ansible:devel`` is frozen will end up in the new collections.

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

Q: What will versioning and deprecation look like for Collections
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* In ansible/ansible:

  * There is a single version number which is over everything shipped in Ansible
  * Doesn't use semver, uses X.Y (ie 2.9) is the major number
  * deprecations are done over 4 versions (~ 2 years)
* In Collections

  * Can be versioned and released independently to Ansible
  * MUST be use `semver (Semantic Versioning) <https://semver.org/>`_

Details around versioning and deprecation policy are still being worked on, we will have a proposal up shortly

Q: How can I fix bugs in Ansible 2.9?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The `previous policy <https://docs.ansible.com/ansible/latest/community/development_process.html#making-your-pr-merge-worthy>`_ was:
1. PR for bug fix including ``changelog/fragment`` file
2. PRs gets merged into ``devel``
3. Backport (``git cherry-pick -x``) PR against the ``stable-2.9`` branch


Once content has been removed from the ``devel`` branch, the process will be:
1. PR for bug fix made against the Collection
2. PR gets merged into Collection
3. Raise PR directly against ``ansible/ansible:stable-2.9`` (ie not a backport) including a ``changelog/fragment`` file

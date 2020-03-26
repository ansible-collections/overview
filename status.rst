**********************************
Ansible Collections Project Status
**********************************

.. contents:: Topics

Overview
========

This page gives an overview

**Warning:** Subject to frequent updates
       This is a "living document", expect it to change as we progress with the Collections work over the next few months.

For background on collections see `Ansible Collections Overview <https://github.com/ansible-collections/overview/blob/master/README.rst>`_.

Summary
=======


A lot of the new Ansible Collection repos are open for contributions
Most Collections aren't ready to be used yet, though you can start contributing to a few.

* Devel is now ``ansible-base``
* Collection repos created: **DONE**

* Collection repos that are ready for use:

  * `amazon.aws <https://github.com/ansible-collections/amazon.aws>`_ done
  * `ansible.posix <https://github.com/ansible-collections/ansible.posix/>`_ done
  * `ansible.windows <https://github.com/ansible-collections/ansible.windows/>`_ done
  * `community.aws <https://github.com/ansible-collections/community.aws>`_ done
  * `community.grafana <https://github.com/ansible-collections/grafana>`_ done
  * `community.kubernetes <https://github.com/ansible-collections/kubernetes>`_ done
  * `community.mongodb <https://github.com/ansible-collections/mongodb>`_ done
  * `community.windows <https://github.com/ansible-collections/community.windows/>`_ done


  
Remaining big ticket items
===========================

The following section details the remaining large blocks of work remaining before Ansible 2.10 can be released.
Some of these item are proposals, or work in progress (WIP) Pull Requests. As Collecions is a large change to development, community and end-users it's really important to us that 

Collection redirection/tombstoning
-----------------------------------

This PR to ansible-base is one of the key features to allow Ansible 2.9 playbooks work without modification in Ansible 2.10

Once this PR merged CI for the ``community.general`` collection repository should be a lot greener.

The main features it will provide are:

* Modules and plugins to be reference by the "short form", ie ``mail`` rather than ``community.general.mail``
* Ability to have deprecated content in Collections via a ``meta/routing.yml`` file in a collection `example <https://github.com/ansible-collections/community.general/blob/master/meta/routing.yml>`_

See ` collection redirection/tombstoning (#67684) <https://github.com/ansible/ansible/pull/67684>`_ for a full list of features.


Versioning and Deprecation policy
---------------------------------

In Ansible 2.9 (and earlier) all modules and plugins were all packaged to gether, so a single version could be used. As collections can be released and versioned independently this is no longer true.

`Versioning and deprecation proposal <https://github.com/ansible-collections/overview/issues/37>`_


ACD Proposal
------------

`ACD Proposal <https://github.com/ansible-collections/overview/issues/39>`_

FIXME: Link to proposal



Automatic push to Galaxy on Git tag
-----------------------------------

Allows Collection owners to publish their Collection to Galaxy with ease.


community.general collection
----------------------------

The ``community.general`` is the largest collection we have. It has ~1,000 modules in.

* Move out ``plugins/modules/network`` into a new ``commuity.network`` collection
* Remove hacks once ` collection redirection/tombstoning (#67684) <https://github.com/ansible/ansible/pull/67684>`_ has been merged
* re-enable disabled integration tests

Ansibulbot
----------

For larger Collection repos we will need ansibulbot to help maintainers.

We expect ansibulbot to be needed on:

* community.aws
* community.general
* community.network
* community.windows

The bot is mostly functional, we need to do some more testing on it


Moving Issues and PRs from ansible/ansible
------------------------------------------

* We have a script which will help move PRs from ansible/ansible into the collection which hosts the code
* Once the repos are stable we will auto close PRs in ansible/ansible
* We don't have a good method to move Issues, these will stay in ansible/ansible for longer


Other items
-----------

This document is just a summary of the bigger todo items.

A list of items can be found in the `Community Collection Project Board <https://github.com/orgs/ansible-collections/projects/1>`_


Limitations and known issues
============================

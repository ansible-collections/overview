*****************************************************
Ansible Community Package Collections Removal Process
*****************************************************

.. contents:: Topics

Overview
========

Sometimes the Ansible community removes a collection from the Ansible package. We try to do this only rarely. This document describes why we might remove a collection from the `Ansible community package <https://pypi.org/project/ansible/>`_ (`build data <https://github.com/ansible-community/ansible-build-data/>`_).

In cases of emergency (for example, a serious security vulnerability that is not fixed in a collection) the `Ansible Community Engineering Steering Committee <https://github.com/ansible/community-docs/blob/main/ansible_community_steering_committee.rst>`_ can vote on emergency exceptions. In most cases, we follow the rules listed on this page.

General processes
=================

This section describes some general processes that will be referred to below.

.. _announce_removal:

Announcing upcoming removal
---------------------------

#. Announce upcoming removal in the Ansible changelog (``https://github.com/ansible-community/ansible-build-data/blob/main/<X>/changelog.yaml``).
   See the following link for an `example on how to add changelog entries to the Ansible changelog <https://github.com/ansible-community/ansible-build-data/pull/68/files>`__.
#. Announce upcoming removal in the collection's issue tracker.
#. Announce upcoming removal in The Bullhorn.

.. _remove_collection:

Removing a collection
---------------------

To remove a collection from Ansible version X.0.0:

#. Remove from ``ansible.in`` (``https://github.com/ansible-community/ansible-build-data/blob/main/<X>/ansible.in``).
#. Remove from ``collection-meta.yaml`` (``https://github.com/ansible-community/ansible-build-data/blob/main/<X>/collection-meta.yaml``).
#. Document actual removal for the first/next alpha of Ansible X.0.0 in the Ansible changelog (``https://github.com/ansible-community/ansible-build-data/blob/main/<X>/changelog.yaml``).
   See the following link for an `example on how to add changelog entries to the Ansible changelog <https://github.com/ansible-community/ansible-build-data/pull/68/files>`__.

.. _readd_collection:

Re-adding a collection
----------------------

Re-adding a collection to Ansible works the same as adding it in the first place. See the `process of adding a new collection to Ansible <https://github.com/ansible-community/ansible-build-data/#adding-a-new-collection>`_ for reference.

#. Re-add collection to ``ansible.in`` (``https://github.com/ansible-community/ansible-build-data/blob/main/<X>/ansible.in``).
#. Re-add collection to ``collection-meta.yaml`` (``https://github.com/ansible-community/ansible-build-data/blob/main/<X>/collection-meta.yaml``).
#. If the removal was announced in the Ansible changelog for a version that has not yet been released (``https://github.com/ansible-community/ansible-build-data/blob/main/<X>/changelog.yaml``), remove the announcement.

Broken collections
==================

The community can remove a collection from the Ansible community package if the collection is broken.

Identifying and removing a broken collection
--------------------------------------------

Conditions for removal
~~~~~~~~~~~~~~~~~~~~~~

A collection is considered broken if one of the following conditions is true:

#. It depends on another collection included in X.0.0 but does not work with the actual version of it that is included, and there is no content in the collection that still works.

We remove broken collections from Ansible (X+1).0.0 under the following conditions:

#. The collection seems to be unmaintained and nobody fixes the problems.
#. The plan to remove the collection in the next major Ansible release is publicized at least two months before the (X+1).0.0 release, and at least one month before the first (X+1).0.0 beta release (feature freeze).

Process
~~~~~~~

The announcement mentioned below must state the reasons for the proposed removal and alert maintainers and the Ansible community that, to prevent the removal, the collection urgently needs new maintainers who can fix the problems.

#. Announce upcoming removal in Ansible X+1 as described in :ref:`announce_removal`.
#. Remove collection from Ansible X+1 as described in :ref:`remove_collection`.

Cancelling removal of a broken collection
-----------------------------------------

Conditions
~~~~~~~~~~

#. The issues have to be fixed and a new release (bugfix, minor or major) has to be made before the Ansible X+1 feature freeze.
#. Someone has to promise to maintain the collection and prevent a similar situation at least for some time.

Process
~~~~~~~

#. Update the removal issue in the collection's issue tracker and close the issue.
#. Announce cancelled removal in The Bullhorn.
#. Re-add collection to Ansible X+1 as described in :ref:`readd_collection`.

Re-adding collection to Ansible
-------------------------------

Conditions
~~~~~~~~~~

Conditions under which the collections can be re-included in the Ansible package without going through the `full inclusion process <https://github.com/ansible-collections/ansible-inclusion/>`_:

#. The issues have to be fixed and a new release has to be made before the Ansible X+2 feature freeze.
#. Someone has to promise to maintain the collection and prevent a similar situation at least for some time.

Process
~~~~~~~

#. Follow `regular process of adding a new collection to Ansible <https://github.com/ansible-community/ansible-build-data/#adding-a-new-collection>`_.

Unmaintained collections
========================

Identifying and removing an unmaintained collection
---------------------------------------------------

Conditions for removal
~~~~~~~~~~~~~~~~~~~~~~

A collection is considered unmaintained if multiple of the following conditions are satisfied:

#. There has been no maintainer's activity in the collection repository for several months (for example, pull request merges and releases).
#. CI has stopped passing (or even has not been running) for several months.
#. Bug reports and bugfix PRs start piling up without being reviewed.

There is no complete formal definition of an unmaintained collection.

Process
~~~~~~~

#. The appearance that the collection is no longer maintained and might be removed from the Ansible package has to be announced both in The Bullhorn and in the collection's issue tracker.
#. At least four weeks after the notice appeared in The Bullhorn and the collection's issue tracker, the Ansible Community Engineering Steering Committee (SC) must look at the collection and vote that it considers it unmaintained. The vote must be open for at least one week.
#. If the SC does not votes that the collection seems to be unmaintained, the process is stopped. The issue needs to be updated accordingly.
#. If X.0.0 will be released next, set Y=X+1. If X.0.0 has already been released, but (X+1).0.0 has not yet been released, set Y=X+2.
#. Announce upcoming removal from Ansible Y as described in :ref:`announce_removal`.
#. Remove collection from Ansible Y as described in :ref:`remove_collection`.

Cancelling removal of an unmaintained collection
------------------------------------------------

Conditions
~~~~~~~~~~

#. Ansible Y has not yet been released.
#. One or multiple maintainers step up, or return, to clean up the collection's state.
#. There have been concrete results made by new maintainers (for example, CI has been fixed, the collection has been released, pull request authors have got meaningful feedback).

Process
~~~~~~~

#. The Steering Committee votes on whether the result is acceptable.
#. A negative vote must come with a good explanation why the clean up work has not been sufficient. In that case, this process stops.
#. If the Steering Committee does not vote against still removing the collection (this includes the case that the vote did not reach quorum), proceed as follows.
#. Re-add collection to Ansible Y as described in :ref:`readd_collection`.

Re-adding collection to Ansible
-------------------------------

There is no simplified process. Once the collection has been removed from Ansible Y.0.0, it needs to go through the full inclusion process to be re-added to the Ansible package. Exceptions are only possible if the Steering Committee votes on them, and it is in the discretion of the Steering Committee to deny a fast re-entry without going through the full review process.

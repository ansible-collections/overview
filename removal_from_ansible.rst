*****************************************************
Ansible Community Package Collections Removal Process
*****************************************************

.. contents:: Topics

Overview
========

This document describes under which circumstances collections can be removed from the `Ansible community package <https://pypi.org/project/ansible/>`_ (`build data <https://github.com/ansible-community/ansible-build-data/>`_). Right now this document tries to set up some first processes. In cases of emergency the `Ansible Community Engineering Steering Committee <https://github.com/ansible/community-docs/blob/main/ansible_community_steering_committee.rst>`_ can vote on emergency exceptions, but the goal is to only use processes documented in here.

Removing broken collections
===========================

If it turns out that a collection contained in Ansible X.0.0 depends on another collection included in X.0.0 and does not work with it, the collection can be removed from Ansible (X+1).0.0 under the following conditions:

1. The collection seems to be unmaintained, and nobody successfully manages to fix the problems.
2. The imminent removal in the next major Ansible release is announced in the Ansible changelog, in The Bullhorn, and in the collection's issue tracker at least two months before the (X+1).0.0 release, and at least one month before the first (X+1).0.0 beta release.

Conditions under which the collection can stay in the Ansible package before removal in Ansible X+1:

1. The issues have to be fixed and a new release (bugfix, minor or major) has to be made before the Ansible X+1 feature freeze. Also someone has to promise to maintain the collection and prevent a similar situation at least for some time.

Conditions under which the collections can be re-included in the Ansible package without going through the full inclusion process (https://github.com/ansible-collections/ansible-inclusion/):

1. The issues have to be fixed and a new release has to be made before the Ansible X+2 feature freeze. Also someone has to promise to maintain the collection and prevent a similar situation at least for some time.

Removing unmaintained collections
=================================

A collection is considered unmaintained if multiple of the following conditions are satisfied:

1. There has been no activity in the collection repository for several months.
2. CI has stopped passing (or even has not been running) for several months.
3. Bug reports and bugfix PRs start piling up without being reviewed or merged.

There is no complete formal definition of what it means that a collection is considered unmaintained. To start the process of removing an unmaintained collection, the following has to be done in the following order:

1. The appearance that the collection is no longer maintained and might be removed from the Ansible package has to be announced both in The Bullhorn and in the collection's issue tracker.
2. The Ansible Community Engineering Steering Committee (SC) has to look at the collection and vote that it considers it unmaintained. This must happen at least four weeks after the notice has appeared in the collection's issue tracker and in The Bullhorn, and the vote must be open for at least one week.
3. If the SC votes that the collection seems to unmaintained, another announcement is made in the collection's issue tracker, in The Bullhorn, and in the Ansible changelog that it will be removed from Ansible Y release. Here Y must be X+1 if Ansible X.0.0 will be released next, or Y must be X+1 if Ansible X.0.0 has already been released. This means that the announcement of impending removal will be active for at least one major release cycle of Ansible.

Conditions under which the collection can stay in the Ansible package before removal in Ansible X+1:

1. One or multiple maintainers step up, or return, to clean up the collection's state. The Steering Committee will vote on whether the result is acceptable. A negative vote must come with a good explanation why the clean up work has not been sufficient.

Once the collection has been removed from Ansible Y.0.0, it needs to go through the full inclusion process to be re-added to the Ansible package. Exceptions are only possible if the Steering Committee votes on them, and it is in the discretion of the Steering Committee to deny a fast re-entry without going through the full review process.

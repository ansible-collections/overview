This will be created as an issue in all collection repositories mentioned in https://github.com/ansible-community/ansible-build-data/blob/main/2.10/ansible.in once it's done.


# Inclusion of this collection in Ansible 2.10

This collection will be included in Ansible 2.10 because it contains modules and/or plugins that were included in Ansible 2.9. Please review:

* The list of requirements for inclusion in Ansible: https://github.com/ansible-collections/overview/blob/master/collection_requirements.rst
* The roadmap with all important dates for Ansible 2.10: https://github.com/ansible/ansible/blob/devel/docs/docsite/rst/roadmap/COLLECTIONS_2_10.rst

## DEADLINE: 2020-08-18

The latest version of the collection available on August 18 will be included in Ansible 2.10.0, except possibly newer versions which differ only in the patch level. (For details, see [the roadmap](https://github.com/ansible/ansible/blob/devel/docs/docsite/rst/roadmap/COLLECTIONS_2_10.rst)). Please release version 1.0.0 of your collection by this date! **If 1.0.0 does not exist, the same 0.x.y version will be used in all of Ansible 2.10 without updates,** and your 1.x.y release will not be included until Ansible 2.11 (unless you request an exception at a [community working group meeting](https://github.com/ansible/community/issues/539) and go through a demanding manual process to vouch for backwards compatibility . . . you want to avoid this!).

## Follow semantic versioning rules

Your collection versioning must follow all [semver rules](https://semver.org/). This means:

* Patch level releases can only contain bugfixes;
* Minor releases can contain new features, new modules and plugins, and bugfixes, but must not break backwards compatibility;
* Major releases can break backwards compatibility.

## Changelogs and Porting Guide

Your collection should provide fragments for the Ansible 2.10 changelog and porting guide. The changelog and porting guide are automatically generated from ansible-base, and from the changelogs of the included collections. All fragments marked `breaking_changes`, `major_changes`, `removed_features` and `deprecated_features` will appear in both the changelog and the porting guide. You have two options for providing changelog fragments to include:

  #. If possible, use the [antsibull-changelog tool](https://github.com/ansible-community/antsibull-changelog/), which uses the same changelog fragment as the ansible/ansible repository (see the [documentation](https://github.com/ansible-community/antsibull-changelog/blob/main/docs/changelogs.rst)).
  #. If you cannot use antsibull-changelog, you can provide the changelog in a machine-readable format as `changelogs/changelog.yaml` inside your collection (see the [documentation of changelogs/changelog.yaml format](https://github.com/ansible-community/antsibull-changelog/blob/main/docs/changelog.yaml-format.md)).

If you cannot contribute to the integrated Ansible changelog using one of these methods, please provide a link to your collection's changelog by creating an issue in https://github.com/ansible-community/ansible-build-data/. If you do not provide changelog fragments or a link, users will not be able to find out what changed in your collection from the Ansible changelog and porting guide.

## Make sure your collection passes the sanity tests

Run `ansible-test sanity --docker -v` in the collection with the latest [ansible-base](https://pypi.org/project/ansible-base/) or `stable-2.10` ansible/ansible checkout.

## Keep informed

Be sure you're subscribed to:

* [Changes impacting Collections](https://github.com/ansible-collections/overview/issues/45) to track changes that Collection maintainers should be aware of;
* The Bullhorn, a newsletter for the Ansible developer community, [back issues and how to add content](https://github.com/ansible/community/issues/546).

## Questions and Feedback

If you have questions or want to provide feedback, please see [the Feedback section in the collection requirements](https://github.com/ansible-collections/overview/blob/master/collection_requirements.rst#feedback).

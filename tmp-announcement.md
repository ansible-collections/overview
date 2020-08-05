This will be created as an issue in all collection repositories mentioned in https://github.com/ansible-community/ansible-build-data/blob/main/2.10/ansible.in once it's done.


# Inclusion of this collection in Ansible 2.10

This collection will be included in Ansible 2.10 because it contains modules and/or plugins that were included in Ansible 2.9. The following links provide you with information on the requirements for inclusion in Ansible and for the current roadmap.

* A complete list of requirements for inclusion into Ansible can be found here: https://github.com/ansible-collections/overview/blob/master/collection_requirements.rst
* The roadmap with all important dates for Ansible 2.10 can be found here: https://github.com/ansible/ansible/blob/devel/docs/docsite/rst/roadmap/COLLECTIONS_2_10.rst

The next sections contain the more important points for now.

## Important date: 2020-08-18

The latest version of the collection by this date will be used in Ansible 2.10.0, except possibly newer versions which differ only in the patch level. (For details, see [the roadmap](https://github.com/ansible/ansible/blob/devel/docs/docsite/rst/roadmap/COLLECTIONS_2_10.rst)). In particular, by this date, version 1.0.0 of your collection should be released. **If not, the same 0.x.y version will be used in all of Ansible 2.10 without updates,** and a 1.x.y will only be included in Ansible 2.11 (except if you can vouch for backwards compatibility with the included version, and then it's a manual process we really want to avoid).

## Your collection versioning must stick to the semantic versioning rules

This means:

* Patch level releases can only contain bugfixes;
* Minor releases can contain new features, new modules and plugins, and bugfixes, but must not break backwards compatibility;
* Major releases can break backwards compatibility.

The detailed rules can be found [here](https://semver.org/).

## Changelogs and Porting Guide

For Ansible 2.10, the changelog and porting guide are automatically generated from the changelog and porting guide of ansible-base, and from the changelogs of the included collections.

If possible, you should use the [antsibull-changelog tool](https://github.com/ansible-community/antsibull-changelog/), which uses the same changelog fragment as the ansible/ansible repository (see the [documentation](https://github.com/ansible-community/antsibull-changelog/blob/main/docs/changelogs.rst)). Alternatively, you can provide the changelog in a machine-readable format as `changelogs/changelog.yaml` inside your collection (see the [documentation of changelogs/changelog.yaml format](https://github.com/ansible-community/antsibull-changelog/blob/main/docs/changelog.yaml-format.md)). For such changelogs, the content of the sections `breaking_changes`, `major_changes`, `removed_features` and `deprecated_features` will be also used for the Ansible porting guide.

These two methods are the only way to integrate your changelog into the Ansible changelog and porting guide. If you do not want to use them, you can provide us with a link to your changelog, so we can link to it from the Ansible changelog. To provide a link, please create an issue in https://github.com/ansible-community/ansible-build-data/. If you do not provide a link, users will not be able to find out what changed in your collection from the Ansible changelog and porting guide.

## Make sure your collection passes the sanity tests

Run `ansible-test sanity --docker -v` in the collection with the latest [ansible-base](https://pypi.org/project/ansible-base/) or `stable-2.10` ansible/ansible checkout.

## Keep informed

Be sure you're subscribed to:

* [Changes impacting Collections](https://github.com/ansible-collections/overview/issues/45) to track changes that Collection maintainers should be aware of;
* The Bullhorn, a newsletter for the Ansible developer community, [back issues and how to add content](https://github.com/ansible/community/issues/546).

## Questions and Feedback

If you have questions or want to provide feedback, please see [the Feedback section in the collection requirements](https://github.com/ansible-collections/overview/blob/master/collection_requirements.rst#feedback).

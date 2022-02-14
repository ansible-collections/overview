# Ansible Collections Checklist (short version)

_For details about the following points, refer to the [Collection Requirements](https://github.com/ansible-collections/overview/blob/main/collection_requirements.rst)._

Every comment should say whether the reviewer expects it to be addressed, or whether it's optional.

**Public availability and communication:**
- [ ] published on [Ansible Galaxy](https://galaxy.ansible.com) with version 1.0.0 or later
- [ ] has a policy of releasing, versioning and deprecation announced to contributors and users in some way
- [ ] have a Code of Conduct (CoC) compatible with the [Ansible Code of Conduct (CoC)](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html)
- [ ] have a publicly available issue tracker that does not require a paid level of service to create an account or view issues
- [ ] have a public SCM repository, releases of the collection are tagged in this repository

**Standards and documentation:**
- [ ] adheres to [semantic versioning](https://semver.org/)
- [ ] follows [licensing rules](https://github.com/ansible-collections/overview/blob/main/collection_requirements.rst#licensing)
- [ ] follows the [Ansible documentation standards](https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_documenting.html) and the [style guide](https://docs.ansible.com/ansible/devel/dev_guide/style_guide/index.html#style-guide)
- [ ] follows [development conventions](https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_best_practices.html); as well as these other requirements:
  - [ ] modules that only gather information are named `<something>_info`
  - [ ] modules that return `ansible_facts` are named `<something>_facts` and do not return non-facts
  - [ ] other modules must not allow querying information using specific `state` option values, or similar mechanisms (like `state=get` or `state=query`).  These features should be moved to `<something>_info` or `<something>_fact` modules.
  - [ ] `check_mode` is supported in all `_info` and `_facts` modules
- [ ] supports Python 2.6 or greater and Python 3.5 or greater. If it does not, read the [full guidelines](https://github.com/ansible-collections/overview/blob/main/collection_requirements.rst#python-compatibility) to see if you qualify for an exception and document the unsupported [Python versions](https://docs.ansible.com/ansible/latest/dev_guide/developing_python_3.html#ansible-and-python-3) in the collection ``README.md`` and in every module and plugin (or in doc fragments)
- [ ] only uses the [allowed plugin types](https://github.com/ansible-collections/overview/blob/main/collection_requirements.rst#modules-plugins) in the `plugins/` directory
- [ ] has `README.md`
- [ ] documentation, examples, and return sections use FQCNs for the `M(..)` [format macros](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_documenting.html#linking-and-other-format-macros-within-module-documentation) when referring to modules
- [ ] modules (or plugins) from ansible-core use `ansible.builtin.` as a FQCN prefix
- [ ] documentation and return sections use `version_added:` containing the *collection* version for which an option, module or plugin was added
- [ ] FQCNs are used for `extends_documentation_fragment:`, unless you are referring to doc_fragments from ansible-core

**Collection management:**
- [ ] `galaxy.yml` having `tags` field set
- [ ] collection dependencies must have a lower bound on the version which is at least 1.0.0, and are all part of the `ansible` package
- [ ] `meta/runtime.yml` defines the minimal version of Ansible which the collection works with
- [ ] has changelog, preferably with `changelogs/changelog.yaml`

**Tests:**
- [ ] passed `ansible-test sanity`
- [ ] if `test/sanity/ignore*.txt` exists, it does not contain error codes listed [here](https://github.com/ansible-collections/overview/blob/main/collection_requirements.rst#ci-testing)
- [ ] has unit or integration tests
- [ ] has CI tests up and running against each of the "major versions" of `ansible-base`/`ansible-core` that the collection supports
- [ ] all CI tests run against every pull request
- [ ] all CI tests run against a commit that releases the collection; if they don't pass, the collection won't be released
- [ ] all CI tests run regularly (nightly, or at least once per week)

# Ansible Collections Checklist (short version)

_For details about the following points, refer to the [Collection Requirements](https://github.com/ansible-collections/overview/blob/main/collection_requirements.rst)._

Every comment should say whether the reviewer expects it to be addressed, or whether it's optional.

**Public availability and communication:**
- [ ] published on Ansible Galaxy [Ansible Galaxy](https://galaxy.ansible.com) with version 1.0.0 or later
- [ ] has a policy of releasing, versioning and deprecation announced to contributors and users in some way
- [ ] have a Code of Conduct (CoC) compatible with the [Ansible Code of Conduct (CoC)](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html)
- [ ] have a publicly available issue tracker that does not require a paid level of service to create an account or view issues
- [ ] have a public SCM repository, releases of the collection are tagged in this repository

**Standards and documentation:**
- [ ] adheres to [semantic versioning](https://semver.org/)
- [ ] follows licensing rules
- [ ] follows the [Ansible documentation standards](https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_documenting.html) and the [style guide](https://docs.ansible.com/ansible/devel/dev_guide/style_guide/index.html#style-guide)
- [ ] follows [development conventions](https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_best_practices.html)
- [ ] only uses allowed plugin types in the `plugins/` directory
- [ ] has `README.md`
- [ ] documentation, examples, and return use FQCNs for `M(..)`, examples, and `seealso` subsections
- [ ] modules (or plugins) from ansible-base use `ansible.builtin.` as a FQCN prefix
- [ ] documentation and return sections use `version_added:` containing the *collection* version for which an option, module or plugin was added
- [ ] FQCNs is used for `extends_documentation_fragment:`, unless you are referring to doc_fragments from ansible-base

**Collection management:**
- [ ] `galaxy.yml` having `tags` field set
- [ ] collection dependencies must have a lower bound on the version which is at least 1.0.0, and are all part of the `ansible` package
- [ ] `meta/runtime.yml` defines the minimal version of Ansible which the collection works with
- [ ] has changelog, preferably with `changelogs/changelog.yaml`
- [ ] collection Galaxy artifact (tarball) should not contain any large objects (binaries) comparatively to the current tarball size limit of 20 MB
- [ ] any objects in a collection Galaxy artifact including the mentioned above must follow [licensing rules](https://github.com/ansible-collections/overview/blob/main/collection_requirements.rst#licensing)

**Tests:**
- [ ] passed `ansible-test sanity`
- [ ] if `test/sanity/ignore*.txt` exists, it does not contain error codes listed [here](https://github.com/ansible-collections/overview/blob/main/collection_requirements.rst#ci-testing)
- [ ] has CI tests up and running against each of the "major versions" of `ansible-base`/`ansible-core` that the collection supports
- [ ] all CI tests run against every pull request
- [ ] all CI tests run against a commit that releases the collection; if they don't pass, the collection won't be released
- [ ] all CI tests run regularly (nightly, or at least once per week)

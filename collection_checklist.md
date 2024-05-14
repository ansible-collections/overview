# Ansible Collections Checklist (short version)

_For details about the following points, refer to the [Collection Requirements](https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html)._

Every comment should say whether the reviewer expects it to be addressed, or whether it's optional.

Note for reviewers: If you don't know how to check any of the points below, please ask maintainers of the collection you're reviewing or a [Steering Committee member](https://docs.ansible.com/ansible/devel/community/steering/community_steering_committee.html#current-steering-committee-members) for clarifications in comments of corresponding inclusion discussion.

**Public availability and communication:**
- [ ] published on [Ansible Galaxy](https://galaxy.ansible.com) with version 1.0.0 or later
- [ ] have a Code of Conduct (CoC) compatible with the [Ansible Code of Conduct (CoC)](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html)
- [ ] have a publicly available issue tracker that does not require a paid level of service to create an account or view issues
- [ ] have a public git repository
- [ ] releases of the collection are tagged in its repository

**Standards and documentation:**
- [ ] adheres to [semantic versioning](https://semver.org/)
- [ ] follows [licensing rules](https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html#collection-licensing-requirements)
- [ ] follows the [Ansible documentation standards](https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_documenting.html) and the [style guide](https://docs.ansible.com/ansible/devel/dev_guide/style_guide/index.html#style-guide)
- [ ] follows [development conventions](https://docs.ansible.com/ansible/devel/dev_guide/developing_modules_best_practices.html); as well as these other requirements:
  - [ ] modules satisfy the concept of [idempotency](https://docs.ansible.com/ansible/latest/reference_appendices/glossary.html#term-Idempotency>)
  - [ ] modules that only gather information are named `<something>_info`
  - [ ] modules that return `ansible_facts` are named `<something>_facts` and do not return non-facts
  - [ ] other modules must not allow querying information using specific `state` option values, or similar mechanisms (like `state=get` or `state=query`).  These features should be moved to `<something>_info` or `<something>_fact` modules.
  - [ ] `check_mode` is supported in all `_info` and `_facts` modules
- [ ] supports [all Python versions supported by all ansible-core versions its supports](https://docs.ansible.com/ansible/devel/reference_appendices/release_and_maintenance.html#ansible-core-support-matrix). If it does not, read the [full guidelines](https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html#python-compatibility) to see if you qualify for an exception and document the unsupported [Python versions](https://docs.ansible.com/ansible/devel/reference_appendices/release_and_maintenance.html#ansible-core-support-matrix) in the collection ``README.md`` and in every module and plugin (or in doc fragments)
- [ ] only uses the [allowed plugin types](https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html#modules-plugins) in the `plugins/` directory
- [ ] has `README.md`
- [ ] FQCNs are used for all plugins and modules including `ansible.builtin.` for builtin ones from ansible-core in all their appearances in the documentation, examples, and return sections, and in `extends_documentation_fragment:`s.
- [ ] documentation and return sections use `version_added:` containing the *collection* version for which an option, module or plugin was added (except cases when they were added in the very first release of the collection)
- [ ] public plugins, roles and playbooks do not use files outside of `meta/`, `plugins/`, `roles/`, and `playbooks/`

**Collection management:**
- [ ] `galaxy.yml` having `tags` field set
- [ ] collection dependencies must have a lower bound on the version which is at least 1.0.0, and are all part of the `ansible` package
- [ ] `meta/runtime.yml` defines the minimal version of Ansible which the collection works with
- [ ] has changelog, preferably with `changelogs/changelog.yaml`
- [ ] collection repository should not contain any large objects (binaries) comparatively to the current Galaxy tarball size limit of 20 MB. For example, package installers for testing purposes shouldn't be added.
- [ ] collection repository should not contain any unnecessary files like, for example, temporary files. Temporary files should be added to `.gitignore`.

**Tests:**

Note for reviewers: If you don't know how to check the points below, please ask maintainers of the collection you're reviewing how you can do it.
* In most cases, CI is set up via GitHub Actions.
* Check `.yml` files in the `.github/workflows` directory. There must be at least the `sanity` section under `jobs` containing the `ansible-test sanity` command running against all supported ansible-core versions that must be also listed there, for example, `- stable-2.15, - stable-2.16, - stable-2.17`.
* Check workflow runs by clicking the `Actions` tab in the repository's page - you're interested in `Scheduled` runs, runs against release commits and runs against pull requests.
* If there are no workflows in the `Actions` tab, ask the collection maintainers how CI is implemented.

- [ ] passed `ansible-test sanity`
- [ ] if `test/sanity/ignore*.txt` exists, it MUST not contain error codes listed in the [list of errors that must not be ignored](https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html#ci-testing)
- [ ] has CI tests up and running against each of the "major versions" of `ansible-base`/`ansible-core` that the collection supports
- [ ] all CI tests MUST run against every pull request
- [ ] all CI tests MUST run regularly (nightly, or at least once per week)
- [ ] sanity tests MUST run against a commit that releases the collection; if they don't pass, the collection won't be released

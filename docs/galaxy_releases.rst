******************************
Ansible Galaxy Release Process
******************************

.. contents:: Topics

Overview
========
In this document we will cover common recommended ways to automate your Ansible
Collection release process.

**Warning:** Subject to frequent updates
       This is a "living document", expect it to change as we progress with the
       Collections work over the next few months.

GitHub Action Workflow for Collection Release
=============================================

Ansible Galaxy does not allow us to use the web-hook we have grown accustom to
for publishing or releasing an Ansible Collection. To publish your Ansible
Collection to GitHub you have two options:

#.  Uploading the :code:`tar.gz` that was created during the build command
    :code:`ansible-galaxy collection build`
#.  By using the :code:`ansible-galaxy collection publish` command after the
    build command.

Ansible Collection Build Process
--------------------------------


Create the :code:`build/` directory.
::

  build
  ├── galaxy_deploy.yml
  └── templates
      └── galaxy.yml.j2

Instead of having the :code:`galaxy.yml` file in our root, we will be generating
this each time we create a release.

You will need to create the :code:`galaxy_deploy.yml` Ansible playbook which
will handle the build and publish steps for us. It's contents should look
something like this.

galaxy_deploy.yml
^^^^^^^^^^^^^^^^^
.. code-block:: yaml

  ---
  - hosts: localhost
    connection: local
    gather_facts: false
    vars:
      # This should be set via the command line at runtime.
      tag: "{{ github_tag.split('/')[-1] }}"
    pre_tasks:
      - name: Ensure the ANSIBLE_GALAXY_TOKEN environment variable is set.
        fail:
          msg: ANSIBLE_GALAXY_TOKEN is not set.
        when: "lookup('env','ANSIBLE_GALAXY_TOKEN') | length == 0"
      - name: Ensure the ~/.ansible directory exists.
        file:
          path: ~/.ansible
          state: directory
      - name: Write the Galaxy token to ~/.ansible/galaxy_token
        copy:
          content: |
            token: {{ lookup('env','ANSIBLE_GALAXY_TOKEN') }}
          dest: ~/.ansible/galaxy_token
          mode: '0600'
    tasks:
      - name: Template out the galaxy.yml file.
        template:
          src: templates/galaxy.yml.j2
          dest: ../galaxy.yml
        register: galaxy_yml
      - name: Build the collection. # noqa 503
        command: >
          ansible-galaxy collection build
          chdir=../
        when: galaxy_yml.changed
      - name: Publish the collection. # noqa 503
        command: >
          ansible-galaxy collection publish ./namespace-collection-{{ tag }}.tar.gz
          chdir=../
        when: galaxy_yml.changed

You'll then need to create the :code:`build/templates` directory which we will
be placing our :code:`galaxy.yml.j2` jinja template. Please edit the jinja
template to your liking. Do not change the :code:`version` section as this is
necessary for Ansible to properly template your :code:`galaxy.yml` for the
specific release.

galaxy.yml.j2
^^^^^^^^^^^^^
.. code-block:: yaml

  ### REQUIRED

  # The namespace of the collection. This can be a company/brand/organization or product namespace under which all
  # content lives. May only contain alphanumeric lowercase characters and underscores. Namespaces cannot start with
  # underscores or numbers and cannot contain consecutive underscores
  namespace: namespace_name

  # The name of the collection. Has the same character restrictions as 'namespace'
  name: collection_name

  # The version of the collection. Must be compatible with semantic versioning
  version: "{{ tag }}"

  # The path to the Markdown (.md) readme file. This path is relative to the root of the collection
  readme: README.md

  # A list of the collection's content authors. Can be just the name or in the format 'Full Name <email> (url)
  # @nicks:irc/im.site#channel'
  authors:
    - "Author1"
    - "Author2 (https://author2.example.com)"
    - "Author3 <author3@example.com>"


  ### OPTIONAL but strongly recommended

  # A short summary description of the collection
  description: Description of what this collection has

  # Either a single license or a list of licenses for content inside of a collection. Ansible Galaxy currently only
  # accepts L(SPDX,https://spdx.org/licenses/) licenses. This key is mutually exclusive with 'license_file'
   license:
     - GPL-2.0-or-later

  # The path to the license file for the collection. This path is relative to the root of the collection. This key is
  # mutually exclusive with 'license'
  # license_file: ''

  # A list of tags you want to associate with the collection for indexing/searching. A tag name has the same character
  # requirements as 'namespace' and 'name'
  tags: []

  # Collections that this collection requires to be installed for it to be usable. The key of the dict is the
  # collection label 'namespace.name'. The value is a version range
  # L(specifiers,https://python-semanticversion.readthedocs.io/en/latest/#requirement-specification). Multiple version
  # range specifiers can be set and are separated by ','
  dependencies: {}

  # The URL of the originating SCM repository
  repository: https://github.com/my_org/my_collection

  # The URL to any online docs
  documentation: https://github.com/my_org/my_collection

  # The URL to the homepage of the collection/project
  homepage: https://github.com/my_org/my_collection

  # The URL to the collection issue tracker
  issues: https://github.com/my_org/my_collection/issues


Configure GitHub Workflow
-------------------------

To be able to execute the Ansible playbook we need to configure the GitHub
Actions Workflow. To do this we will need to create the
:code:`.github/workflows/` folder if it does not already exist. Then create a
new file :code:`release.yml` and it should look something like this.

.. code-block:: yaml

  name: "release"
  on:
  release:
    types:
      - created
  jobs:
  release:
    runs-on: ubuntu-18.04
    env:
      ANSIBLE_GALAXY_TOKEN: ${{ secrets.ANSIBLE_GALAXY_TOKEN }}
      ANSIBLE_FORCE_COLOR: 1
    steps:
      - name: Check out code
        uses: actions/checkout@v1

      - name: Set up Python 3.8
        uses: actions/setup-python@v1
        with:
          python-version: 3.8

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install ansible

      - name: Run the Ansible Galaxy deploy playbook
        run: >-
          ansible-playbook -i 'localhost,' build/galaxy_deploy.yml -e "github_tag=${{ github.ref }}"

In this file you may notice we have :code:`github.ref`, and
:code:`secrets.ANSIBLE_GALAXY_TOKEN` vars. These vars will be handled by GitHub
on execution of the workflow. The :code:`github.ref` is automatic and will be
parsed from GitHub as it passes this value on release to the Workflow
automatically.

Configure ANSIBLE_GALAXY_TOKEN Secret
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#.  Go to Ansible Galaxy and get your API Key. You can find it on
    https://galaxy.ansible.com/me/preferences in the section API Key. Just click
    on "Show API Key".
#.  Go to your GitHub repository. Click on "Settings" -> "Secrets".
#.  Click on "Add a new secret"
#.  For the name of the secret, it's important you use ANSIBLE_GALAXY_TOKEN
    as that is what the Workflow is referencing. These values must match
#.  After you place the API Key as the value, click on "Add secret".

After this step is completed you should be able to trigger a release and watch
your Ansible collection be published on the GitHub Actions page.

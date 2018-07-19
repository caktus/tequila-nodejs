tequila-nodejs
==============

This repository holds an `Ansible <http://www.ansible.com/home>`_ role
that is installable using ``ansible-galaxy``.  This role depends upon
`geerlingguy/nodejs
<https://github.com/geerlingguy/ansible-role-nodejs>`_ and contains
tasks used to build the front-end assets for a Django web app.  It
exists primarily to support the `Caktus Django project template
<https://github.com/caktus/django-project-template>`_.

More complete documenation can be found in `caktus/tequila
<https://github.com/caktus/tequila>`_.


License
-------

This Ansible role is released under the BSD License.  See the `LICENSE
<https://github.com/caktus/tequila-nodejs/blob/master/LICENSE>`_ file for
more details.


Contributing
------------

If you think you've found a bug or are interested in contributing to
this project check out `tequila-nodejs on Github
<https://github.com/caktus/tequila-nodejs>`_.

Development sponsored by `Caktus Consulting Group, LLC
<http://www.caktusgroup.com/services>`_.


Dependencies
------------

- `geerlingguy/nodejs <https://github.com/geerlingguy/ansible-role-nodejs>`_


Installation
------------

Create an ``ansible.cfg`` file in your project directory to tell
Ansible where to install your roles (optionally, set the
``ANSIBLE_ROLES_PATH`` environment variable to do the same thing, or
allow the roles to be installed into ``/etc/ansible/roles``) ::

    [defaults]
    roles_path = deployment/roles/

Create a ``requirements.yml`` file in your project's deployment
directory.  It is recommended to include `tequila-common
<https://github.com/caktus/tequila-common>`_, which sets up the
project directory structure and users; `tequila-django
<https://github.com/caktus/tequila-django>`_, which checks out your
project and installs it as a Django web app; and also include and
explicitly pin the version of `geerlingguy/nodejs
<https://github.com/geerlingguy/ansible-role-nodejs>`_, which this
role depends on to install nodejs and any front-end packages that your
project requires ::

    ---
    # file: deployment/requirements.yml
    - src: https://github.com/caktus/tequila-common
      version: v0.8.0

    - src: https://github.com/caktus/tequila-django
      version: v0.9.11

    - src: geerlingguy.nodejs
      version: 4.1.2
      name: nodejs

    - src: https://github.com/caktus/tequila-nodejs
      version: v0.8.0

Run ``ansible-galaxy`` with your requirements file ::

    $ ansible-galaxy install -r deployment/requirements.yml

or, alternatively, run it directly against the url ::

    $ ansible-galaxy install git+https://github.com/caktus/tequila-nodejs

The project then should have access to the ``tequila-nodejs`` role in
its playbooks.


Variables
---------

The following variables are used by the ``tequila-nodejs`` role:

- ``project_name`` **required**
- ``root_dir`` **default:** ``"/var/www/{{ project_name }}"``
- ``source_dir`` **default:** ``"{{ root_dir }}/src"``
- ``project_user`` **default:** ``"{{ project_name }}"``
- ``ignore_devdependencies`` **default:** ``false``
- ``nodejs_package_json_path`` **required**, typically should be ``"{{
  source_dir }}"``
- ``npm_run_command`` **default** ``build`` - tequila-nodejs runs
  ``npm run build`` by default, this lets you change that to run
  a different *run* command.

Due to `some <https://github.com/npm/npm/issues/17471>`_ `issues
<https://github.com/ansible/ansible/issues/29234>`_ discovered with
npm not managing package installation when new packages are added to
the ``devDependencies`` object in package.json, tequila-nodejs checks
for the presence of any packages in this variable and will throw an
error if found.  This behavior can be disabled by setting
``ignore_devdependencies`` to ``true``.  Any such packages found
should almost certainly be moved to the ``dependencies`` object
instead.


Notes
-----

See `geerlingguy/nodejs
<https://github.com/geerlingguy/ansible-role-nodejs>`_ for the
expected Ansible configuration variables for that role.

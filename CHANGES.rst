Tequila-nodejs
==============

Changes

v 0.8.2 on Nov 21, 2018
-----------------------

* Updated project dependencies to ensure that project packages
  get installed as the project user. Prior to this release,
  geerlingguy.nodejs would install both Node and the project
  Node dependencies as root, causing permissions issues.
  Now Node and project Node dependencies are installed in
  two separate steps, the former as root, the latter as
  the project user.

v 0.8.1 on Jul 23, 2018
-----------------------

* New variable ``npm_run_command`` **default** ``build`` - tequila-nodejs runs
  ``npm run build`` by default, this lets you change that to run
  a different *run* command.

v 0.8.0 on Apr 26, 2018
-----------------------

* Created this new role by splitting out npm-specific tasks from
  tequila-django.  Projects wanting to upgrade to tequila-django
  v0.9.12 will need to use this role if there is a npm build step
  involved.  These projects will need to add tequila-nodejs to their
  requirements.yml file while still retaining geerlingguy.nodejs,
  which is a dependency of this new role.  Additionally, the
  playbook/web.yml playbook should be updated to execute
  tequila-nodejs instead of geerlingguy.nodejs, and it should be
  placed immediately after the tequila-django role.  See the new
  example web.yml file in the playbooks directory in the
  `caktus/tequila <https://github.com/caktus/tequila>`_ repo.

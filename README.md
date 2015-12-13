Tendenci
=========

Automate installation of Tendenci AMS.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.


Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

`tendenci_install_name must not contain spaces as its used in various places for configuration and in filesystem paths.


Other Configuration
-------------------
To push out an updated admin password remove the file at {{ tendenci_website_install_dir }}/.created_superuser_{{ tendenci_site_administrator_username }} . While the file exists that users password will remain unchanged by ansible.

Dependencies
------------

This role tries to use existing roles to perform generic work wherever
possible. To that end the following roles are used; their defaults (where set)
are in vars/main.yml but they can be configured per their upstream
documentation.

franklinkim.apt
===============
This is used to install packages and will need to be used.
variables used:

bennojoy.memcached
===================
Installs and configures memcached. This could be done manually so can be regarded as optional if unavailable.
variables used:

ANXS.postgresql
===============
Used to perform all database configuration; this role is required.
variables used:

tersmitten.postfix
==================
Perform install and configuration of Postfix. Strictly speaking this is optional.
variables used:


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
        - role: goetzk.tendenci
      vars:
        - tendenci_install_name: 'goetzk_tendenci'
        - tendenci_site_secret_key: 'my 32 character string here asdf234'
        - tendenci_site_settings_key: 'my 32 character string here asdf234'

License
-------

This Ansible role is GNU GPL2+

Author Information
------------------

Issues or feedback can be reported to the author at karl@kgoetz.id.au; please
prefix the subject with 'ansible' or 'role'.


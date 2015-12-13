Tendenci
=========

Automate installation of Tendenci AMS.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

If using SELinux, App Armour or any other security enforcement tools it is your responsibility to ensure the various services are able to communicate.

For Django migrations to run successfully a minimum of 2GB of RAM is required though 3GB+ is recommended.


Example Playbook
----------------

In this instance only the required settings are changed, all others remain at their defaults

    - hosts: servers
      roles:
        - role: goetzk.tendenci
      vars:
        - tendenci_postgres_connection_pass: 'MyS3cur3P4ss'
        - tendenci_site_secret_key: 'my 32 character string here 0982lkj'
        - tendenci_site_settings_key: 'my 32 character string here ;lkj938'
        - tendenci_site_administrator_password: 'S0meth1ngS4f3'
        - tendenci_site_administrator_email: 'you@example.com'
        - tendenci_server_visible_name: '192.168.93.106'


Other Configuration
-------------------
To push out an updated admin password remove the file at {{ tendenci_website_install_dir }}/.created_superuser_{{ tendenci_site_administrator_username }} . While the file exists that users password will remain unchanged by ansible.


Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.


There are some variables which MUST be updated. They are:
- your database connection password
- site secret key
- site key
- admin password
- adminemail address
- tendenci hostname

Failing to update these fields is likely to compromise the security of your site or (in the case of hostname) make it inaccessible. See also the example playbook.

For safety sake ensure`tendenci_install_name does not contain spaces. It is used in various places for things including database and filesytem configuration where spaces could be problematic.


The full ist of variables is extensive and includes a number from the third party roles this package depends on - see the dependencies section for variables handled by those roles.

Dependencies
------------

This role tries to use existing roles to perform  work wherever
possible. To that end the following roles are used; their defaults (where set)
are in defaulst/main.yml but they can be configured per their upstream
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

Changes from upstream install
-----------------------------
This role attempts to remain close to upstreams install though it may drift out of sync over time. Should that occur bug reports are welcome.

Known differences are:
 - migrate_initials.sh isn't run by this role. This is due to the difficulty of getting ansible to run migrate_initials.sh in the correct virtual environment.

Changes from upstream install
-----------------------------
This role attempts to remain close to upstreams install though it may drift out of sync over time. Should that occur bug reports are welcome.

Known differences are:
 - migrate_initials.sh isn't run by this role. This is due to the difficulty of getting ansible to run migrate_initials.sh in the correct virtual environment.

License
-------

This Ansible role is GNU GPL2+

Author Information
------------------

Issues or feedback can be reported to the author at karl@kgoetz.id.au; please
prefix the subject with 'ansible' or 'role'.


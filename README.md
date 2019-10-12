Tendenci
=========

Automate installation of Tendenci Association Management System

This has been tested on Debian 8 and Ubuntu 14.04 but I suspect would work on other releases too.

Requirements
------------

If using SELinux, App Armour or any other security enforcement tools it is your responsibility to ensure the various services are able to communicate.

For Django migrations to run successfully a minimum of 2GB of RAM is required though 3GB+ is recommended.

All packages required for operation should be installed by this role or those it depends on.

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
To push out an updated admin password remove the file at {{ tendenci_website_install_dir }}/.created_superuser_{{ tendenci_site_administrator_username }} . While the file exists that users password will remain unchanged by Ansible.


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

    # Name of this tendenci install, used in filesystem paths and postgresql table names
    tendenci_install_name: "tendenci"
    # For example these paths use it for cooinstallability but you can choose different paths.
    tendenci_virtualenv_install_dir: "/srv/{{ tendenci_install_name }}/environment"
    tendenci_website_install_dir: "/srv/{{ tendenci_install_name }}/website"

    # Password will be encrypted by postgresql during user creation
    tendenci_postgres_connection_pass: "Good strong password.... dont forget."
    tendenci_postgres_database_name:   "{{ tendenci_install_name }}_db"

    # These are upstream default values, make sure you change them!
    tendenci_site_secret_key: "your_unique_secret_key_Qoh222VG9pq8P9hOapH"
    tendenci_site_settings_key: "tendenci_site_key_bdc635k2-283d-4a2c-a477-339ea866"

    # Details of the first administrator/superuser account
    tendenci_site_administrator_username: "admin"
    tendenci_site_administrator_email: "admin@example.com"
    # As with the site keys above, be sure to change this.
    tendenci_site_administrator_password: "admin"

    # Set false to prevent all nginx configuration (eg nginx configuration is custom/external)
    tendenci_perform_nginx_configuration: true

    # DNS name of server
    tendenci_server_visible_name: localhost
    tendenci_server_visible_port: 80

    # Where django will bind to, note that this is an internal address and
    # users see the _server_visible_* values above.
    tendenci_bind_address: 127.0.0.1
    tendenci_bind_port: 8000

    # If you have a custom theme, put its name here
    tendenci_custom_theme_name: ""

    # User and group for the daemons to run as
    tendenci_run_as_user: www-data
    tendenci_run_as_group: www-data


Dependencies
------------

This role tries to use existing roles to perform work wherever
possible. To that end the following roles are used; their defaults (where set)
are in defaulst/main.yml but they can be configured per their upstream
documentation.

franklinkim.apt
===============
This is used to install packages and will need to be used. The default package set is listed below.

    apt_packages:
     - git
     - python-dev           # for pip to build compiled modules against
     - libjpeg-dev          # for image manipulation
     - python-pip           # manage install process for python modules
     - virtualenv           # create virtual environment for python packages to be installed in
     - build-essential      # provide packages needed for compilation (used by pip)
     - libmemcached-dev     # enable linking against memcache for pip
     - nginx                # front end web server
     - sudo                 # Needed by ANX.postgresql.

bennojoy.memcached
===================
Installs and configures memcached. This could be done manually so can be regarded as optional if unavailable.

    memcached_cache_size: 256       # The cache size in MB
    memcached_listen_address: 127.0.0.1 # IP to listen on

ANXS.postgresql
===============
Used to perform all database configuration; this role is required.

    postgresql_version: 9.4
    postgresql_encoding: 'UTF-8'
    postgresql_locale: 'en_AU.UTF-8'
    postgresql_lc_messages: 'en_AU.UTF-8'
    postgresql_lc_monetary: 'en_AU.UTF-8'
    postgresql_lc_numeric: 'en_AU.UTF-8'
    postgresql_lc_time: 'en_AU.UTF-8'

    postgresql_admin_user: "postgres"
    postgresql_default_auth_method: "trust"

    postgresql_ext_install_contrib: true
    postgresql_ext_install_dev_headers: true
    postgresql_ext_install_postgis: true

    postgresql_databases:
      - name: "{{ tendenci_postgres_database_name }}"
        owner: "{{ postgres_connection_user }}"

    postgresql_users:
      - name: "{{ postgres_connection_user }}"
        pass: "{{ tendenci_postgres_connection_pass }}"
        encrypted: no       # denotes if the password is already encrypted.

    postgresql_user_privileges:
      - name: "{{ postgres_connection_user }}"
        db: "{{ tendenci_postgres_database_name }}"
        priv: "ALL"
        role_attr_flags: "CREATEDB"

Changes from upstream install
-----------------------------
This role attempts to remain close to upstreams install though it may drift out of sync over time. Should that occur bug reports are welcome.

Known differences are:
 - migrate_initials.sh isn't run by this role. This is due to the difficulty of getting Ansible to run migrate_initials.sh in the correct virtual environment.

License
-------

This Ansible role is GNU GPL2+

Author Information
------------------

Issues or feedback can be reported to the author at karl@kgoetz.id.au; please
prefix the subject with 'ansible' or 'role'.


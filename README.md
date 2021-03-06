inmotionhosting.wordpress_ultrastack
=========

Modular Ansible Role for deploying and configuring WordPress, including InMotion's optimized UltraStack

[![Build Status](https://travis-ci.org/inmotionhosting/ansible-role-wordpress_ultrastack.png?branch=master)](https://travis-ci.org/inmotionhosting/ansible-role-wordpress_ultrastack)

Requirements
------------

* CentOS 7.x or later
* Debian 9 or later
* Ubuntu 16.04 LTS or later

Role Variables
--------------

### Defaults
Variables in `defaults/main.yml`

| Variable | Definition |
| -------- | ---------- |
| certbot_create_command | The command to run for generating a LetsEncrypt SSL via certbot.
| certbot_package | The package name to use when installing certbot.
| certbot_stop_services | The servers that must be stopped when installing an SSL.
| pass_gen_alias | A variable acting as an alias to generate safe passwords.
| site_domain | The domain to associate with the WordPress installation.
| site_email | The email to associated with the WordPress administrative account.
| site_user | The username to associated with the WordPress administrative account.
| site_pass | The password to associated with the WordPress administrative account.
| system_user | The system user that the WordPress site will belong to.
| use_ultrastack | When true, applies a high-performance WordPress configuration.<br><br>Default: `False`
| use_letsencrypt | When true, generates a Let's Encrypt SSL for the target site(s) after confirming the domain properly resolves to the target host.<br><br>Default: `False`.
| wp_db_name | The name of the database to associate with this WordPress installation.
| wp_db_user | The name of the database user account to associate with this WordPress installation.
| wp_db_pass | (Optional) The password to associate with the `wp_db_user`.  If not defined, the password will be automatically generated via `pass_gen_alias`.
| wp_plugins | The list of WordPress plugins to install.  Note that plugins are only installed in fresh deployments of WordPress; changing this list and redeploying will not add/remove plugins fron an existing installation of WordPress.
| wp_system_folder | The name of the folder WordPress should be installed to within the `system_user`'s home directory.
| wp_system_path | The full path to the `system_user`'s `wp_system_folder`.

Dependencies
------------

### Required

```yaml
- role: inmotionhosting.apache
- role: inmotionhosting.mysql
- role: inmotionhosting.php_fpm
```

### Optional
These roles are required when `use_ultrastack: true`:

```yaml
- role: inmotionhosting.nginx_proxy
- role: inmotionhosting.redis
```

Example Playbook
----------------

```yaml
- hosts: wordpress_lamp
  roles:
    - role: inmotionhosting.apache
    - role: inmotionhosting.mysql
    - role: inmotionhosting.php_fpm
    - role: inmotionhosting.wordpress_ultrastack

- hosts: wordpress_ultrastack
  roles:
    - role: inmotionhosting.apache
    - role: inmotionhosting.mysql
    - role: inmotionhosting.php_fpm
    # nginx_proxy and redis are included conditionally from within the
    # wordpress_ultrastack role
    - role: inmotionhosting.wordpress_ultrastack
      vars:
        use_ultrastack: true
```

License
-------

GPLv3

Author Information
------------------

[InMotion Hosting](https://inmotionhosting.com)

# Ansible Role for PHP

Installs PHP and configure php.ini and php pool on CentOS 7, 9 and Ubuntu servers.

Used:

* https://rpms.remirepo.net/
* https://launchpad.net/~ondrej/+archive/ubuntu/php

## Templates

The templates used are the default ones when installing php with remi ()`see templates/`).

## Variables

Available variables are listed below, along with default values `(see defaults/main.yml)`:


### PHP Installation

```
php_version: 8.2
```
Default php version is 8.2, but the value can be overwritten from host_vars.

A list of the PHP packages to install is already declared `see vars/`.

```
php_default_packages: []
```

You can add other packages you'd like (in host_vars):

```
php_extra_packages: []
```

### Configuration php.ini

See `vars`
```
php_ini_path:
```

These variables will be replaced in the php.ini file that exists on
the server at `php_ini_path`

```
php_default_settings:
  - { option: "expose_php", value: "Off", section: "PHP" }
  - { option: "memory_limit", value: "512M", section: "PHP" }
  - { option: "max_execution_time", value: "180", section: "PHP" }
  - { option: "max_input_time", value: "60", section: "PHP" }
  - { option: "max_input_vars", value: "10000", section: "PHP" }
  - { option: "realpath_cache_size", value: "4096k", section: "PHP" }
  - { option: "file_uploads", value: "On", section: "PHP" }
  - { option: "post_max_size", value: "200M", section: "PHP" }
  - { option: "upload_max_filesize", value: "200M", section: "PHP" }
  - { option: "max_file_uploads", value: "20", section: "PHP" }
  - { option: "allow_url_fopen", value: "On", section: "PHP" }
  - { option: "display_errors", value: "Off", section: "PHP" }
  - { option: "display_startup_errors", value: "Off", section: "PHP" }
  - { option: "error_log", value: "syslog", section: "PHP" }
  - { option: "assert.active", value: "On", section: "Assertion" }
  - { option: "date.timezone", value: "UTC", section: "Date" }
  - { option: "mail.add_x_header", value: "0", section: "mail function" }
```

If you want to override or add others variables, use:

```
php_custom_settings: []
```

### PHP-FPM (php pool)

Php fpm is enabled by default, but can be disabled from host_vars.
If this variable is set to `false`, the pool will not be modified and
the `php_fpm_service` will not be started and enabled.

```
php_fpm_enable: true 
```

See `vars`:

```
php_fpm_service: 
```

These variables will be replaced in the pool template (templates/pool.conf-Distribution.j2)

```
php_fpm_pools:
  - name: "www"
    user: nginx
    group: nginx
    listen: "{{ '/var/run/php' + php_version | replace('.', '') + '-php-fpm.sock' }}"
    listen_owner: nginx # listen.owner
    listen_group: nginx # lsiten.group
    listen_mode: 0660 # listen.mode
    pm_max_children: 50 # pm.max_children
    pm_start_servers: 10 # pm.start_servers
    pm_min_spare_servers: 10 # pm.min_spare_servers
    pm_max_spare_servers: 20 # pm.max_spare_servers
    pm_max_requests: 500 # pm.max_requests
```

Defaul pool is `www`. If you want to modify the default settings
for `www` pool or to add a new pool, copy the given list to host_vars and update it.

## Dependencies

None.

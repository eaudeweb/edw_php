# Ansible Role for PHP

Installs PHP and configure php.ini and php pool on CentOS 7 and 9 servers.

## Templates

The templates used are the default ones when installing php with remi (`see templates/`).

## Variables

Available variables are listed below, along with default values `(see defaults/main.yml)`:


### PHP Installation

```
php_version: 8.2
```
Default php version is 8.2, but the value can be overwritten from host_vars.

A list of the PHP packages to install is already declared.

```
php_default_packages:
  - "{{ 'php' + php_version | replace('.', '') }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-cli' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-common' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-devel' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-fpm' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-gd' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-intl' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-mbstring' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-mysqlnd' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pdo' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pear' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pecl-uuid' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pecl-yaml' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-tidy' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-xml' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-runtime' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pecl-imagick' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-opcache' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pecl-memcached' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pecl-mcrypt' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pecl-mysql' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pecl-zip' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-pecl-igbinary' }}"
  - "{{ 'php' + php_version | replace('.', '') + '-php-process' }}"
```

You can add other packages you'd like (in host_vars):

```
php_extra_packages: []
```

### Configuration php.ini

```
php_ini_path: "{{ '/etc/opt/remi/php' + php_version | replace('.', '') + '/php.ini'}}"
```
by default `/etc/opt/remi/php82/php.ini`

These variables will be replaced in the php.ini template (templates/php.ini.j2)

```
php_ini_settings: 
  - allow_url_fopen: "On"
    assert_active: "On"  # sau -1  # assert.active
    date_timezone: "UTC" # date.timezone
    display_errors: "Off"
    display_startup_errors: "Off"
    error_log: "syslog"
    expose_php: "Off"
    file_uploads: "On"
    mail_add_x_header: "0" # mail.add_x_header
    max_execution_time: "180"
    max_file_uploads: "20"
    max_input_time: "60"
    max_input_vars: "10000"
    memory_limit: "512M"
    post_max_size: "200M"
    realpath_cache_size: "4096k"
    upload_max_filesize: "200M"
```

if you want to modify one of the variables, you must copy the
entire given list and update it in host_vars.

### PHP-FPM (php pool)

Php fpm is enabled by default, but can be disabled from host_vars.
If this variable is set to `false`, the pool will not be modified and
the `php_fpm_service` will not be started and enabled.

```
php_fpm_enable: true 
```

```
php_fpm_service: "{{ 'php' + php_version | replace('.', '') + '-php-fpm' }}"
```

These variables will be replaced in the pool template (templates/pool.conf.j2)

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

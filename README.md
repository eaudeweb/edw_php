# Ansible Role for PHP

Installs PHP and configure php.ini and php pool on CentOS 7 and 9 servers.

## Variables

Available variables are listed below, along with default values `(see defaults/main.yml)`:

### PHP Installation

```
php_version: 8.2
```
Default php version is 8.2, but the value can be overwritten from host_vars.

A list of the PHP packages to install is already declared.

```
php_default_packages: []
```

You can add other packages you'd like (in host_vars):

```
php_extra_packages: []
```

### Configuration php.ini

If you don't want to apply the configuration settings provided in `php_default_settings`, you can set the 
next variable to `false` in host_var.

```
apply_php_default_settings: true
``` 

```
php_default_settings: []
```

```
php_ini_path: "{{ '/etc/opt/remi/php' + php_version | replace('.', '') + '/php.ini'}}"
```
by default `/etc/opt/remi/php82/php.ini`

For custom settings:
```
php_extra_settings: []
```
It can be used to overwrite some of the default settings or to add others.

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

If you don't want to apply the configuration settings provided in `php_default_pool`, you can set
the next variable to `false` in host_var.
It can be used to overwrite some of the default settings or to add others.

```
apply_php_default_pool: true
``` 

```
php_default_pool: []
```

```
php_pool_path: "{{ '/etc/opt/remi/php' + php_version | replace('.', '') + '/php-fpm.d/www.conf'}}"
```
by default `/etc/opt/remi/php82/php-fpm.d/www.conf`

For custom settings:
```
php_pool: []
```
## Dependencies

None.

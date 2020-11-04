#  Enigma


### Enable plugin in Roundcube
In `mailcow-dockerized/data/web/roundcube/config/config.inc.php` add it to the `plugins` array:
```php
$config['plugins'] = array(
        'enigma',
);
```

### Create folder for keys
```sh
cd mailcow-dockerized/data/web
mkdir enigma_keys
chmod 777 enigma_keys
chown 82:docker enigma_keys
```

### Plugin config
In `mailcow-dockerized/data/web/roundcube/plugins/enigma/config.inc.php`, set the path:

```php
<?php
// REQUIRED! Keys directory for all users.
// Must be writeable by PHP process, and not in the web server document root
$config['enigma_pgp_homedir'] = '/web/enigma_keys/';
```

### Enable pgp execution
In `mailcow-dockerized/data/conf/phpfpm/php-fpm.d/pools.conf`, remove `proc_open` from this list:
```ini
[web-worker]
php_admin_value[disable_functions] = <remove proc_open from the list>
```

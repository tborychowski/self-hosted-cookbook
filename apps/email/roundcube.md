# Roundcube

It's the best looking, stable, reliable and configurable open source WebMail.
In my setup Roundcube is integrated with MailCow server.

<br>

- [Homepage](https://roundcube.net/)
- [Roundcube in Mailcow](https://mailcow.github.io/mailcow-dockerized-docs/third_party-roundcube/)
- [Github repo](https://github.com/roundcube/roundcubemail)

### Plugins
- [Carddav](carddav.md)
- [Enigma](enigma.md)
- [SMTP identity](https://plugins.roundcube.net/#/packages/elm/identity_smtp) [external] - Send emails from gmail account
- [HTML5 Notifier](https://plugins.roundcube.net/#/packages/kitist/html5_notifier) [external]
- [Easy unsubscribe](https://plugins.roundcube.net/#/packages/ss88/easy_unsubscribe) [external] - (composer require "ss88/easy_unsubscribe @dev")
- [Automatic addressbook](https://plugins.roundcube.net/#/packages/sblaisot/automatic_addressbook) [external]
- [Context menus](https://plugins.roundcube.net/#/packages/johndoh/contextmenu) [external]
- [Plugin installer](https://plugins.roundcube.net/#/packages/roundcube/plugin-installer) [external]
- [Folder size](https://plugins.roundcube.net/#/packages/jfcherng/show-folder-size) [external]
- [Filters](https://plugins.roundcube.net/#/packages/roundcube/filters) [external]


## mailcow-dockerized/data/web/roundcube/config/config.inc.php
```php
<?php
error_reporting(0);
if (!file_exists('/tmp/mime.types')) {
    file_put_contents("/tmp/mime.types", fopen("http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types", 'r'));
}

$config = array();
$config['db_dsnw'] = 'mysql://' . getenv('DBUSER') . ':' . getenv('DBPASS') . '@mysql/' . getenv('DBNAME');
$config['default_host'] = 'tls://dovecot';
$config['default_port'] = '143';
$config['smtp_server'] = 'tls://postfix';
$config['smtp_port'] = 587;
$config['smtp_user'] = '%u';
$config['smtp_pass'] = '%p';
$config['support_url'] = '';
$config['product_name'] = 'Roundcube Webmail';
$config['des_key'] = 'yourrandomstring_changeme';
$config['log_dir'] = '/dev/null';
$config['temp_dir'] = '/tmp';
$config['plugins'] = array(
	'archive',
	'enigma',
	'zipdownload',
	'password',
	'carddav',
	'html5_notifier',
	'identity_smtp',
	'managesieve',
	'markasjunk'
);
$config['skin'] = 'elastic';

$config['mime_types'] = '/tmp/mime.types';
$config['imap_conn_options'] = array(
  'ssl' => array('verify_peer' => false, 'verify_peer_name' => false, 'allow_self_signed' => true)
);
$config['enable_installer'] = true;
$config['smtp_conn_options'] = array(
  'ssl' => array('verify_peer' => false, 'verify_peer_name' => false, 'allow_self_signed' => true)
);

$config['managesieve_port'] = 4190;
$config['managesieve_host'] = 'tls://dovecot';
$config['managesieve_conn_options'] = array(
  'ssl' => array('verify_peer' => false, 'verify_peer_name' => false, 'allow_self_signed' => true)
);
// Enables separate management interface for vacation responses (out-of-office)
// 0 - no separate section (default),
// 1 - add Vacation section,
// 2 - add Vacation section, but hide Filters section
$config['managesieve_vacation'] = 1;
$config['db_prefix'] = 'mailcow_rc1';

$config['address_book_type'] = '';

// Session lifetime in minutes, 10080min = 7 days
$config['session_lifetime'] = 10080;
```


## Tips & Tricks

### "Remember me" session
- [Issue reference](https://github.com/roundcube/roundcubemail/issues/5050#issuecomment-377663569)

1. In `mailcow-dockerized/data/web/roundcube/program/lib/Roundcube/rcube.php`
    replace:
    ```php
    ini_set('session.cookie_lifetime', 0);
    ```
    with:
	```php
    ini_set('session.cookie_lifetime', 2592000); // 1 month
	```


2. In `mailcow-dockerized/data/web/roundcube/program/lib/Roundcube/rcube_session.php`
    replace:
	```php
	rcube_utils::setcookie($this->cookiename, $this->cookie, 0);
	```
    with:
	```php
	$timestamp_in_one_month = time() + 60 * 60 * 24 * 30;
    rcube_utils::setcookie($this->cookiename, $this->cookie, $timestamp_in_one_month);
	```

### Enable logging
In `mailcow-dockerized/data/web/roundcube/config/config.inc.php` add:
```php
$config['log_dir'] = '/web/roundcube/logs';
```


### Upgrade script
```sh
#!/bin/bash

V=1.4.9

echo "Upgrading Roundcube to v.$V..."
wget "https://github.com/roundcube/roundcubemail/releases/download/$V/roundcubemail-$V.tar.gz"
tar -xvf "roundcubemail-$V.tar.gz"
rm "roundcubemail-$V.tar.gz"

cd "roundcubemail-$V"
sudo ./bin/installto.sh /var/www/mailcow-dockerized/data/web/roundcube
cd ..
rm -rf "roundcubemail-$V"
echo "Done."
echo "Remember to update the session cookie expiry!"
```

# Mailcow

Probably the best email server solution for self-hosting.
Features:
- allows fetching from other imap servers (like gmail)
- alias emails, alias domains, temporary aliases
- integrated SoGo out-of-the-box, and can be integrated with other webmails (Roundcube, Rainloop)
- With Roundcube plugin you can send emails through 3rd party imap

<br>

- [Homepage](https://mailcow.email/)
- [Github repo](https://github.com/mailcow/mailcow-dockerized)
- [Docs](https://mailcow.github.io/mailcow-dockerized-docs/)


## Setup
```sh
git clone https://github.com/mailcow/mailcow-dockerized
cd mailcow-dockerized
./generate_config.sh
nano mailcow.conf
```

## mailcow.conf
```ini
# ------------------------------
# mailcow web ui configuration
# ------------------------------
# example.org is _not_ a valid hostname, use a fqdn here.
# Default admin user is "admin"
# Default password is "moohoo"
MAILCOW_HOSTNAME=mail.example.com

# ------------------------------
# SQL database configuration
# ------------------------------
DBNAME=mailcow
DBUSER=mailcow

# Please use long, random alphanumeric strings (A-Za-z0-9)
DBPASS=<PASSWORD>
DBROOT=<PASSWORD>

# ------------------------------
# HTTP/S Bindings
# ------------------------------
# You should use HTTPS, but in case of SSL offloaded reverse proxies:
# Might be important: This will also change the binding within the container.
# If you use a proxy within Docker, point it to the ports you set below.

HTTP_PORT=7080
HTTP_BIND=0.0.0.0
HTTPS_PORT=7443
HTTPS_BIND=0.0.0.0

# ------------------------------
# Other bindings
# ------------------------------
# You should leave that alone
# Format: 11.22.33.44:25 or 0.0.0.0:465 etc.
# Do _not_ use IP:PORT in HTTP(S)_BIND or HTTP(S)_PORT
SMTP_PORT=25
SMTPS_PORT=465
SUBMISSION_PORT=587
IMAP_PORT=143
IMAPS_PORT=993
POP_PORT=110
POPS_PORT=995
SIEVE_PORT=4190
DOVEADM_PORT=127.0.0.1:19991
SQL_PORT=127.0.0.1:13306
SOLR_PORT=127.0.0.1:18983

# Your timezone
TZ=Etc/UTC

# Fixed project name
COMPOSE_PROJECT_NAME=mailcowdockerized

# Set this to "allow" to enable the anyone pseudo user. Disabled by default.
# When enabled, ACL can be created, that apply to "All authenticated users"
# This should probably only be activated on mail hosts, that are used exclusivly by one organisation.
# Otherwise a user might share data with too many other users.
ACL_ANYONE=disallow

# Garbage collector cleanup
# Deleted domains and mailboxes are moved to /var/vmail/_garbage/timestamp_sanitizedstring
# How long should objects remain in the garbage until they are being deleted? (value in minutes)
# Check interval is hourly
MAILDIR_GC_TIME=1440

# Additional SAN for the certificate
#
# You can use wildcard records to create specific names for every domain you add to mailcow.
# Example: Add domains "example.com" and "example.net" to mailcow, change ADDITIONAL_SAN to a value like:
#ADDITIONAL_SAN=imap.*,smtp.*
# This will expand the certificate to "imap.example.com", "smtp.example.com", "imap.example.net", "imap.example.net"
# plus every domain you add in the future.
#
# You can also just add static names...
#ADDITIONAL_SAN=srv1.example.net
# ...or combine wildcard and static names:
#ADDITIONAL_SAN=imap.*,srv1.example.com
ADDITIONAL_SAN=

# Skip running ACME (acme-mailcow, Let's Encrypt certs) - y/n
SKIP_LETS_ENCRYPT=y

# Create seperate certificates for all domains - y/n
# this will allow adding more than 100 domains, but some email clients will not be able to connect with alternative hostnames
# see https://wiki.dovecot.org/SSL/SNIClientSupport
ENABLE_SSL_SNI=n

# Skip IPv4 check in ACME container - y/n
SKIP_IP_CHECK=n

# Skip HTTP verification in ACME container - y/n
SKIP_HTTP_VERIFICATION=n

# Skip ClamAV (clamd-mailcow) anti-virus (Rspamd will auto-detect a missing ClamAV container) - y/n
SKIP_CLAMD=n

# Skip Solr on low-memory systems or if you do not want to store a readable index of your mails in solr-vol-1.
SKIP_SOLR=n

# Solr heap size in MB, there is no recommendation, please see Solr docs.
# Solr is a prone to run OOM and should be monitored. Unmonitored Solr setups are not recommended.
SOLR_HEAP=1024

# Enable watchdog (watchdog-mailcow) to restart unhealthy containers (experimental)
USE_WATCHDOG=n

# Allow admins to log into SOGo as email user (without any password)
ALLOW_ADMIN_EMAIL_LOGIN=n

# Send notifications by mail (sent from watchdog@MAILCOW_HOSTNAME)
# CAUTION:
# 1. You should use external recipients
# 2. Mails are sent unsigned (no DKIM)
# 3. If you use DMARC, create a separate DMARC policy ("v=DMARC1; p=none;" in _dmarc.MAILCOW_HOSTNAME)
# Multiple rcpts allowed, NO quotation marks, NO spaces

#WATCHDOG_NOTIFY_EMAIL=a@example.com,b@example.com,c@example.com
WATCHDOG_NOTIFY_EMAIL=

# Notify about banned IP (includes whois lookup)
WATCHDOG_NOTIFY_BAN=y

# Checks if mailcow is an open relay. Requires a SAL. More checks will follow.
# https://www.servercow.de/mailcow?lang=en
# https://www.servercow.de/mailcow?lang=de
# No data is collected. Opt-in and anonymous.
# Will only work with unmodified mailcow setups.
WATCHDOG_EXTERNAL_CHECKS=n

# Max log lines per service to keep in Redis logs
LOG_LINES=9999

# Internal IPv4 /24 subnet, format n.n.n (expands to n.n.n.0/24)
IPV4_NETWORK=172.16.1

# Internal IPv6 subnet in fc00::/7
IPV6_NETWORK=fd4d:6169:6c63:6f77::/64

# Use this IPv4 for outgoing connections (SNAT)
#SNAT_TO_SOURCE=

# Use this IPv6 for outgoing connections (SNAT)
#SNAT6_TO_SOURCE=

# Create or override API key for web ui
# You _must_ define API_ALLOW_FROM, which is a comma separated list of IPs
# API_KEY allowed chars: a-z, A-Z, 0-9, -
#API_KEY=
#API_ALLOW_FROM=172.22.1.1,127.0.0.1

# mail_home is ~/Maildir
MAILDIR_SUB=Maildir

# SOGo session timeout in minutes
SOGO_EXPIRE_SESSION=480

REDIS_PORT=127.0.0.1:7654
# Skip SOGo: Will disable SOGo integration and therefore webmail, DAV protocols and ActiveSync support (experimental, unsupported, not fully implemented) - y/n
SKIP_SOGO=n
# Create or override read-only API key for web UI
#API_KEY_READ_ONLY=
```

Login with `admin`:`moohoo`



## Upgrading

There's `update.sh` script in the cloned repo:
```sh
sudo ./update.sh

# Check for updates
sudo ./update.sh --check

# Do not start mailcow after applying an update
sudo ./update.sh --skip-start

# Update with merge strategy "ours" instead of "theirs"
# This will merge in favor for your local changes.
sudo ./update.sh --ours

# Don't update, but prefetch images and exit
sudo ./update.sh --prefetch
```

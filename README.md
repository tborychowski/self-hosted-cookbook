# Self-hosted Cookbook

If you're like me and love not only to self-host, but to constantly test new apps, you probably already discovered `docker-compose` as the fastest and easiest way towards that goal.
There is, however, one problem: not all image authors are as great as [linuxserver.io](https://hub.docker.com/u/linuxserver), whose docs are as simple as they should be: you just copy & paste `docker-compose.yml` and run `docker-compose up -d` and IT JUST WORKS!
This is how all images should be documented!
But unfortunately, it isn't!<br>
Sometimes you have to spend a lot of time to make it work.<br>
Hence - this repo.<br>
The aims is to provide a ready-to-run recipes that you can just copy, paste and run.<br>
So, without further ado, here's the current list:

# How to use this cookbook
- There are certain things that some recipes need which cannot be filled in due to security reasons.
  - `example.com` needs to be replaced with your own domain
  - `username`, `password`, etc. - should be replaced by your username & password
  - keys (like `APP_KEY`, `SECRET` etc.) should be regenerated using e.g. `openssl rand -base64 32`
- Not all apps have been tested & described. These are marked as 🔗 (external links).


# General Information
- [Get started with docker & docker-compose](docker/get-started.md)
- [Troubleshooting](docker/troubleshooting.md)

# Other self-hosted sources
- [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted)
- [r/selfhosted](https://www.reddit.com/r/selfhosted/)
- [Homelab OS](https://homelabos.com/docs/#available-software)


# Ad Blockers & local DNS
- [AdGuard Home](apps/ad-blockers/adguard.md)
- [PiHole](apps/ad-blockers/pihole.md)
- [Block Lists](apps/ad-blockers/lists.md)


# Antivirus
- [MalwareMultiScan](https://github.com/mindcollapse/MalwareMultiScan) 🔗


# Backup
- [Duplicati](apps/backup/duplicati.md)
- [Elkar Backup](apps/backup/elkar-backup.md)
- [websync](https://furier.github.io/websync/) 🔗 - an rsync task manager, where tasks can be added, scheduled and maintained in a sane manner


# Blogging
- [AnchorCMS](https://github.com/anchorcms/anchor-cms#installation) 🔗
- [Ghost](https://ghost.org/docs/install/docker/) 🔗
- [Hugo](https://gohugo.io/) 🔗
- [Kirby](https://getkirby.com/) 🔗
- [Metalsmith](https://metalsmith.io/) 🔗
- [Pagekit](https://pagekit.com/docs/getting-started/installation) 🔗
- [Pelican](https://docs.getpelican.com/en/stable/quickstart.html#installation) 🔗
- [PostLeaf](https://www.postleaf.org/installing) 🔗
- [Textpattern](https://docs.textpattern.com/installation/) 🔗
- [WriteFreely](https://github.com/writeas/writefreely) 🔗


# Bookmarks & Read Later
- [Shaarli](apps/bookmarks/shaarli.md)
- [Shiori](apps/bookmarks/shiori.md)
- [Shaark](apps/bookmarks/shaark.md)
- [Wallabag](apps/bookmarks/wallabag.md)


### Other
- [Nunux Keeper](https://keeper.nunux.org/) 🔗 - similar to wallabag, but not as good (more complicated, less usable and doesn't have mobile apps).


# Cloud & File Sharing
- [FileRun](apps/cloud/filerun.md)
- [NextCloud](apps/cloud/nextcloud.md)
- [Pydio](apps/cloud/pydio.md)
- [Seafile](apps/cloud/seafile.md)


# Cookbook
- [NextCloud Cookbook](https://apps.nextcloud.com/apps/cookbook) 🔗 - quite good. Can import from URL (some pages), but manually editing longer recipes is a bit of a pain (you need to add and paste every single ingredient & preparation step one-by-one).
- [recipes](https://github.com/vabene1111/recipes) 🔗 - a bit complex, but feature rich food processing manager for your home (from shopping to the table). Importing doesn't seem to work as good as in the NextCloud's Cookbook (for some pages at least).


# Dashboard
- [DashMachine](apps/dashboard/dashmachine.md)
- [Homer](apps/dashboard/homer.md)
- [SUI](apps/dashboard/sui.md)
- [Organizr](https://github.com/causefx/Organizr) 🔗
- [Heimdall](https://github.com/linuxserver/Heimdall) 🔗


# Docker Managers
- [Diun](apps/docker/diun.md)
- [WatchTower](apps/docker/watch-tower.md)


# Download Managers
- [Deluge](apps/downloads/deluge.md)
- [qbittorrent](apps/downloads/qbit.md)
- [Transmission](apps/downloads/transmission.md)


# E-mail
- CLIENTS (webmail)
  - [Roundcube](apps/email/roundcube.md)
  - [Rainloop](http://www.rainloop.net/) 🔗
  - [Rainloop in MailCow](https://github.com/mailcow/mailcow-dockerized/issues/613) 🔗
  - [Mailpile](https://www.mailpile.is/) 🔗
  - [WebMail Lite](https://afterlogic.com/docs/webmail-lite-8/installation) 🔗
  - [Cypht](https://cypht.org/) 🔗
  - [Cypht docker](https://hub.docker.com/r/sailfrog/cypht-docker) 🔗
- SERVERS
  - [Mailcow](apps/email/mailcow.md)
  - [Mailu](https://github.com/Mailu/Mailu) 🔗 - Can't send from roundcube as e.g. `username@gmail.com`
  - [Mail-in-a-box](https://mailinabox.email/) 🔗
  - [Mailcare](https://gitlab.com/mailcare/mailcare) 🔗 - open source disposable email address service.
  - [Poste.io](https://poste.io/doc/getting-started) 🔗 - doesn't allow fetching from other imap servers, keeps pushing the pro version
  - [Wildduck](https://wildduck.email/#/) 🔗 - has app passwords, doesn't seem to have contacts or other stuff (unless you'd use e.g. Roundcube)
- Hosted e-mail providers:
  - [Migadu](https://www.migadu.com/) 🔗 - No calendar, just email with a contactbook. Swiss-based. From: $19/year.
  - [Mailcheap](https://www.mailcheap.co/email-shared.html) 🔗 - Sogo & Afterlogic with calendar. USA based. No "delete account". From: $24/year.
- SMTP Relays
  - [SMTP2Go](https://www.smtp2go.com/pricing/) 🔗 - Free: 1k/month
  - [Mailgun](https://www.mailgun.com/pricing/) 🔗 - PAYG: $8/10k/month
  - [Sendgrid](https://sendgrid.com/pricing/) 🔗 - Free 100/day
  - [MailJet](https://www.mailjet.com/pricing/) 🔗 - Free 6k/month
- Anonymous emails - not self-hosted but important for privacy
  - [Reddit Thread](https://www.reddit.com/r/selfhosted/comments/isu8mw/selfhosted_throw_away_email_addresses_that_allow/) 🔗
  - [burnermail.io](https://burnermail.io/) 🔗
  - [anonaddy.com](https://anonaddy.com/#pricing) 🔗
  - [simplelogin.io](https://simplelogin.io/) 🔗
  - [simplelogin.io github repo](https://github.com/simple-login/app) 🔗
- Tools
  - [verify domain for google](https://postmaster.google.com/managedomains) 🔗
  - [remove IP from spam house](https://www.spamhaus.org/lookup/) 🔗
  - [check dns & reverse dns](https://mxtoolbox.com/) 🔗
  - [reverse-dns-check](https://www.debouncer.com/reverse-dns-check) 🔗
  - [DNS Records checker](https://www.digwebinterface.com/) 🔗
  - [Domain security checker](https://www.hardenize.com/) 🔗


# GIT
- [Gitea](https://docs.gitea.io/en-us/install-with-docker/) 🔗
- [GitLab](https://about.gitlab.com/) 🔗
- [Gogs](https://gogs.io/) 🔗
- [Phabricator](https://secure.phabricator.com/book/phabricator/article/configuration_guide/) 🔗


# Home Automation
- [HomeAssistant](apps/home-automation/home-assistant.md)
- [Beehive](https://github.com/muesli/beehive) 🔗 - flexible event/agent & automation system
- [Huginn](https://github.com/huginn/huginn) 🔗 - Create agents that monitor and act on your behalf.
- [Node-RED](https://nodered.org/) 🔗 - Low-code programming for event-driven applications
- [Kibitzr](https://kibitzr.github.io/) 🔗 - Personal Web Assistant


# Media Managers
- [Bazarr (subtitles)](apps/media/bazarr.md)
- [Calibre (e-books)](apps/media/calibre.md)
- [Deemix](apps/media/deemix.md)
- [Invidious](apps/media/invidious.md)
- [Jackett (search engine proxy/adapter)](apps/media/jackett.md)
- [Komga (comics)](apps/media/komga.md)
- [Navidrome](apps/media/navidrome.md)
- [Radarr (movies)](apps/media/radarr.md)
- [Readerr (ebooks & comics)](apps/media/readerr.md)
- [Sonarr (tv shows)](apps/media/sonarr.md)
- [Tautulli](apps/media/tautulli.md)
- [Ubooquity](http://vaemendis.net/ubooquity) 🔗 - Another Ebook & Comics server. Didn't work properly.
- [Youtube downloader](apps/media/youtube-downloader.md)


# Monitors
### Self-hosted
- [Cachet](apps/monitors/cachet.md)
- [Dockprom](apps/monitors/dockprom.md)
- [PhpServerMonitor](apps/monitors/php-server-monitor.md)
- [Statping](apps/monitors/statping.md)

### Other, not-fully tested
- [Staytus](https://github.com/adamcooke/staytus) 🔗 - service status is updated manually!
- [cstate](https://cstate.mnts.lt/) 🔗 - weird...
- [Glances](https://glances.readthedocs.io/en/stable/install.html) 🔗 - resource hog
- [Netdata](https://hub.docker.com/r/netdata/netdata) 🔗 - lots of stuff, nothing relevant
- [LibreNMS](https://github.com/librenms/docker) 🔗 - ugly

### Hosted
- [statuspage.io](https://www.atlassian.com/software/statuspage) 🔗 - same - manual process!
- [updown](https://updown.io) 🔗 - doesn't seem to have a page with multiple services' statuses...
- [healthchecks](https://healthchecks.io) 🔗 - cron-based monitoring, no public status page, just badges
- [uptimerobot](https://uptimerobot.com) 🔗 - free is very basic, constantly nags for upgrade to paid...

### Useful links
- [awesome-sysadmin: monitoring](https://github.com/n1trux/awesome-sysadmin#monitoring)
- [reddit thread](https://www.reddit.com/r/selfhosted/comments/epzt3f/im_looking_for_a_lean_monitoring_and_altert/)
- [reddit thread](https://www.reddit.com/r/selfhosted/comments/gwe18p/looking_for_a_neat_status_page_like/)



# Notifications
- [Notifiers by service](apps/notifications/notifiers-by-service.md) - comparison table
- [Pushover](apps/notifications/pushover.md)
- [Synology-sms-relay](apps/notifications/synology-sms-relay.md)
- [Synology-notifications](https://github.com/ryancurrah/synology-notifications) 🔗 - similar to the above - works with Slack (and potentially Discord)


### Other
- [apprise](https://github.com/caronc/apprise) 🔗
- [Synapse](https://github.com/matrix-org/synapse#synapse-installation) 🔗
- [Gotify](https://github.com/gotify/server) 🔗 - notification server
- [pushover](https://pushover.net/) 🔗
- [unifi event monitor](https://github.com/tborychowski/unifi-event-monitor) 🔗


# Other services
- [bitwarden_rs](apps/other/bitwarden.md)
- [Cockpit](apps/other/cockpit.md)
- [Code server](apps/other/code.md)
- [Firefox sync server](apps/other/firefox-sync.md)


# Photos
- [Comparison table](apps/photos/comparison.md)
- [PhotoPrism](apps/photos/photoprism.md)
- [PhotoView](apps/photos/photoview.md)
- [Piwigo](apps/photos/piwigo.md)
- [Pixelfed](apps/photos/pixelfed.md)

### Other tested
- [Chevereto](https://chevereto.com/) 🔗 - quite nice. No video support.
- [Lychee](https://lycheeorg.github.io/) 🔗 - good looking, no video support.
- [PhotoShow](https://github.com/thibaud-rohmer/PhotoShow/) 🔗 - seems dead and doesn't work.
- [Photosync](https://www.photosync-app.com/home.html) 🔗 - paid, app, not really self-hosted, just sync.
- [OwnPhotos](https://github.com/hooram/ownphotos) 🔗 - limited features, ugly & dead.
- [FileStash](https://github.com/mickael-kerjean/filestash) 🔗 - old-time-dropbox-like file manager.

### Untested
- [pigallery2](https://github.com/bpatrik/pigallery2) 🔗 - minimal feats
- [photostructure](https://photostructure.com/) 🔗 - too early
- [picapport](https://www.picapport.de/en/index.php) 🔗 - weird

 # Project Management
- [Jira](apps/project-mgmt/jira.md)
- [Kanboard](apps/project-mgmt/kanboard.md)
- [OpenProject](apps/project-mgmt/open-project.md)
- [Planka](apps/project-mgmt/planka.md)
- [Vikunja](apps/project-mgmt/vikunja.md)
- [Wekan](apps/project-mgmt/wekan.md)
- [Taskcafe](https://github.com/JordanKnott/taskcafe) 🔗 - early stage, active development.
- [YouTrack](https://www.jetbrains.com/youtrack/) 🔗 - The project management tool designed for agile teams (from JetBrains).


# Reverse proxy & SSO
- [Authelia](apps/reverse-proxy-sso/authelia.md)
- [Traefik](apps/reverse-proxy-sso/traefik.md)
- [Caddy](https://caddyserver.com/) 🔗 - very good web server with reverse-proxy & automatic https.
- [Nginx Proxy Manager](https://nginxproxymanager.com/) 🔗 - another nice solution based on the battle-tested & probably the most popular web-server - nginx. It has a pretty UI that allows to manage the services.


 # RSS
 - [Miniflux](rss/../apps/rss/miniflux.md)
 - [Miniflux-filter](rss/../apps/rss/miniflux-filter.md)
 - [FreshRSS](https://www.freshrss.org/) 🔗 - second best :-)


### RSS Tools
- [PolitePol](https://github.com/taroved/pol) 🔗 - Create RSS where there was none
- [FetchRSS](https://fetchrss.com/) 🔗 - Create RSS for FB, Twitter, YT, and websites
- [rss-bridge](https://github.com/RSS-Bridge/rss-bridge) 🔗 - The RSS feed for websites missing it
- [rss2full](https://github.com/feedocean/rss2full) 🔗 - Transform summary feeds into full-text


# Search engines
- [Searx](apps/search/searx.md)
- [Whoogle](apps/search/whoogle.md)


# Social
- [Monica](apps/social/monica.md)
- [Etesync](apps/social/etesync.md)

### Other untested
- [HumHub](https://www.humhub.com/en) 🔗 - Free social network software and framework.
- [RocketChat](https://docs.rocket.chat/installation/docker-containers/) 🔗 - The Ultimate Communication Hub.
- [Snikket](https://snikket.org/) 🔗 - Chat that is simple, secure, and private.
- [Jami](https://jami.net/) 🔗 - Audio & video calls, screen sharing, IM.


# Wiki
- [Confluence](apps/wiki/confluence.md)
- [Bookstack](apps/wiki/bookstack.md)
- [Wiki.js](apps/wiki/wikijs.md)
- [XWiki](apps/wiki/xwiki.md)


### Other
- [Pepperminty Wiki](https://peppermint.mooncarrot.space/) 🔗 - wiki engine contained in a single file. Doesn't seem to have a structured navigation (tree-like menu). Subpages are supported though.
- [Wreeto](https://github.com/chrisvel/wreeto_official) 🔗 - impossible to install
- [Outline](https://github.com/chsasank/outline-wiki-docker-compose) 🔗 (Original docker was impossible to use. This one allegedly works.) - It looks cool, but requires Slack to use...
- [Gollum](https://github.com/gollum/gollum) 🔗 - A simple, Git-powered wiki with a sweet API and local frontend.

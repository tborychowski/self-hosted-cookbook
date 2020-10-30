# Self-hosted Cookbook

If you're like me and love not only to self-host, but to constantly test new apps, you probably already discovered `docker-compose` as the fastest and easiest way towards that goal.
There is, however, one problem: not all image authors are as great as [linuxserver.io](https://hub.docker.com/u/linuxserver), whose docs are as simple as they should be: you just copy & paste `docker-compose.yml` and run `docker-compose up -d` and IT JUST WORKS!
This is how all images should be documented!
But unfortunately, it isn't!<br>
Sometimes you have to spend a lot of time to make it work.<br>
Hence - this repo.<br>
The aims is to provide a ready-to-run recipes that you can just copy, paste and run.<br>
So, without further ado, here's the current list:

# General Information
- How to setup docker?
- Troubleshooting


# Ad Blockers & local DNS
- [AdGuard Home](apps/ad-blockers/adguard.md)
- [PiHole](apps/ad-blockers/pihole.md)
- [Block Lists](apps/ad-blockers/lists.md)

# Antivirus
- [MalwareMultiScan](https://github.com/mindcollapse/MalwareMultiScan) [external]

# Blogging
- [AnchorCMS](https://github.com/anchorcms/anchor-cms#installation)  [external]
- [Ghost](https://ghost.org/docs/install/docker/)  [external]
- [Hugo](https://gohugo.io/)  [external]
- [Kirby](https://getkirby.com/)  [external]
- [Metalsmith](https://metalsmith.io/)  [external]
- [Pagekit](https://pagekit.com/docs/getting-started/installation)  [external]
- [Pelican](https://docs.getpelican.com/en/stable/quickstart.html#installation)  [external]
- [PostLeaf](https://www.postleaf.org/installing)  [external]
- [Textpattern](https://docs.textpattern.com/installation/)  [external]
- [WriteFreely](https://github.com/writeas/writefreely)  [external]

# Bookmarks
- [Shaarli](apps/bookmarks/shaarli.md)
- [Shiori](apps/bookmarks/shiori.md)
- [Shaark](apps/bookmarks/shaark.md)

# Cloud & File Sharing
- [FileRun](apps/cloud/filerun.md)
- [NextCloud](apps/cloud/nextcloud.md)
- [Pydio](apps/cloud/pydio.md)
- [Seafile](apps/cloud/seafile.md)

# Dashboard
- [DashMachine](apps/dashboard/dashmachine.md)
- [Homer](apps/dashboard/homer.md)
- [SUI](apps/dashboard/sui.md)
- [Organizr](https://github.com/causefx/Organizr) [external]
- [Heimdall](https://github.com/linuxserver/Heimdall) [external]

# Docker Managers
- [Diun](apps/docker/diun.md)
- [WatchTower](apps/docker/watch-tower.md)

# Download Managers
- [Deluge](apps/downloads/deluge.md)
- [qbittorrent](apps/downloads/qbit.md)
- [Transmission](apps/downloads/transmission.md)

# E-mail
- Clients (webmail)
- Servers
- Hosted e-mail providers
- SMTP Relays
- Anonymous emails
- Tools

# Home Automation
- [HomeAssistant](apps/home-automation/home-assistant.md)
- [Huginn](https://github.com/huginn/huginn) [external]
- [Node-RED](https://nodered.org/) [external]


# Media Managers
- [Bazarr (subtitles)](apps/media/bazarr.md)
- [Calibre (e-books)](apps/media/calibre.md)
- [Deemix](apps/media/deemix.md)
- [Jackett (search engine proxy/adapter)](apps/media/jackett.md)
- [Komga (comics)](apps/media/komga.md)
- [Navidrome](apps/media/navidrome.md)
- [Radarr (movies)](apps/media/radarr.md)
- [Readerr (ebooks & comics)](apps/media/readerr.md)
- [Sonarr (tv shows)](apps/media/sonarr.md)
- [Tautulli](apps/media/tautulli.md)
- [Ubooquity](http://vaemendis.net/ubooquity) [external] - Another Ebook & Comics server. Didn't work properly.
- [Youtube downloader](apps/media/youtube-downloader.md)


# Monitors
### Self-hosted
- [Cachet](apps/monitors/cachet.md)
- [Dockprom](apps/monitors/dockprom.md)
- [PhpServerMonitor](apps/monitors/php-server-monitor.md)
- [Statping](apps/monitors/statping.md)

### Other, not-fully tested
- [Staytus](https://github.com/adamcooke/staytus) [external] - service status is updated manually!
- [cstate](https://cstate.mnts.lt/) [external] - weird...
- [Glances](https://glances.readthedocs.io/en/stable/install.html)  [external] - resource hog
- [Netdata](https://hub.docker.com/r/netdata/netdata)  [external] - lots of stuff, nothing relevant
- [LibreNMS](https://github.com/librenms/docker)  [external] - ugly

### Hosted
- [statuspage.io](https://www.atlassian.com/software/statuspage)  [external] - same - manual process!
- [updown](https://updown.io)  [external] - doesn't seem to have a page with multiple services' statuses...
- [healthchecks](https://healthchecks.io)  [external] - cron-based monitoring, no public status page, just badges
- [uptimerobot](https://uptimerobot.com)  [external] - free is very basic, constantly nags for upgrade to paid...

### Useful links
- [awesome-sysadmin: monitoring](https://github.com/n1trux/awesome-sysadmin#monitoring)
- [reddit thread](https://www.reddit.com/r/selfhosted/comments/epzt3f/im_looking_for_a_lean_monitoring_and_altert/)
- [reddit thread](https://www.reddit.com/r/selfhosted/comments/gwe18p/looking_for_a_neat_status_page_like/)



# Notifications
- [Notifiers by service](apps/notifications/notifiers-by-service.md) - comparison table
- [Pushover](apps/notifications/pushover.md)
- [Synology-sms-relay](apps/notifications/synology-sms-relay.md)


### Other
- [apprise](https://github.com/caronc/apprise) [external]
- [Synapse](https://github.com/matrix-org/synapse#synapse-installation) [external]
- [Gotify](https://github.com/gotify/server) [external] - notification server
- [pushover](https://pushover.net/) [external]
- [unifi event monitor](https://github.com/tborychowski/unifi-event-monitor) [external]



# Photos
- [Comparison table](apps/photos/comparison.md)
- [PhotoPrism](apps/photos/photoprism.md)
- [PhotoView](apps/photos/photoview.md)
- [Piwigo](apps/photos/piwigo.md)
- [Pixelfed](apps/photos/pixelfed.md)

## Other tested
- [Chevereto](https://chevereto.com/) [external] - quite nice. No video support.
- [Lychee](https://lycheeorg.github.io/) [external] - good looking, no video support.
- [PhotoShow](https://github.com/thibaud-rohmer/PhotoShow/) [external] - seems dead and doesn't work.
- [Photosync](https://www.photosync-app.com/home.html) [external] - paid, app, not really self-hosted, just sync.
- [OwnPhotos](https://github.com/hooram/ownphotos) [external] - limited features, ugly & dead.
- [FileStash](https://github.com/mickael-kerjean/filestash) [external] - old-time-dropbox-like file manager.

## Untested
- [pigallery2](https://github.com/bpatrik/pigallery2) [external] - minimal feats
- [photostructure](https://photostructure.com/) [external] - too early
- [picapport](https://www.picapport.de/en/index.php) [external] - weird

 # Project Management
- [Jira](apps/project-mgmt/jira.md)
- [Kanboard](apps/project-mgmt/kanboard.md)
- [OpenProject](apps/project-mgmt/open-project.md)
- [Planka](apps/project-mgmt/planka.md)
- [Vikunja](apps/project-mgmt/vikunja.md)
- [Wekan](apps/project-mgmt/wekan.md)
- [Taskcafe](https://github.com/JordanKnott/taskcafe) [external] - early stage, active development.


# Reverse proxy & SSO
- Authelia
- Traefik


 # RSS
 - Miniflux
 - Miniflux-filter


# Search engines
- Searx
- Whoogle search

# Social
- Monica CRM
- Etesync
- HumHub
- IM


# Wiki
- Confluence
- Bookstack
- Gollum https://github.com/gollum/gollum
- Wiki.js

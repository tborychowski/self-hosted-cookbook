# Self-hosted Cookbook

If you're like me and love not only to self-host, but to constantly test new apps, you probably already discovered `docker-compose` as the fastest and easiest way towards that goal.
There is, however, one problem: not all image authors are as great as [linuxserver.io](https://hub.docker.com/u/linuxserver), whose docs are as simple as they should be: you just copy & paste `docker-compose.yml` and run `docker-compose up -d` and IT JUST WORKS!
This is how all images should be documented!
But unfortunately, it isn't!<br>
Sometimes you have to spend a lot of time to make it work.<br>
Hence - this repo.<br>
The aims is to provide a ready-to-run recipes that you can just copy, paste and run.<br>
So, without further ado, here's the current list:

# Ad Blockers & local DNS
- [AdGuard Home](apps/ad-blockers/adguard.md)
- [PiHole](apps/ad-blockers/pihole.md)
- [Block Lists](apps/ad-blockers/lists.md)


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

# Cloud
- [FileRun](apps/cloud/filerun.md)
- [NextCloud](apps/cloud/nextcloud.md)
- [Pydio](apps/cloud/pydio.md)
- [Seafile](apps/cloud/seafile.md)

# Dashboard
- [DashMachine](apps/dashboard/dashmachine.md)
- [Homer](apps/dashboard/homer.md)
- [SUI](apps/dashboard/sui.md)
- [Organizr](https://github.com/causefx/Organizr) [external]
- [Heimdall](https://github.com/linuxserver/Heimdall)

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
- HomeAssistant

# Media Managers
- Calibre (e-books)
- Deemix
- Komga (comics)
- Navidrome
- Readerr (ebooks & comics)
- Sonarr (tv shows)
- Radarr (movies)
- Bazaar (subtitles)
- Jackett (search engine proxy/adapter)
- Tautulli
- Youtube downloader

# Monitors
- Cachet
- Dockprom
- Statping

# Notifications
- Pushover
- Synology-notifications
- Synology-sms-relay

# Photos
- PhotoPrism
- PhotoView
- Piwigo
- Pixelfed

 # Project Management
- Jira
- Kanboard
- OpenProject
- Planka
- Vikunja
- Wekan

# Reverse proxy & SSO
- Authelia
- Traefik

 # RSS
 - Miniflux (rss)

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
- Wiki.js

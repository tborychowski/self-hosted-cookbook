# Pushover

- As of today, the best notifier out there
- Has mobile app and web desktop client
- Provides email-to-notification service
- Free tier sufficient for a homelab

<br>

- [Homepage](https://pushover.net)
- [Web Client](https://client.pushover.net)
- [Docs](https://pushover.net/api#messages)


## Example script
```sh
#!/bin/bash

# info    #666d7b
# success #408062
# warning #af8a1a
# danger  #8b4848

MSG=$(printf "<font color=\"#408062\">%s</font>" "$@")

curl -s -X POST \
    --data-urlencode "token=<app key>" \
    --data-urlencode "user=<user key>" \
    --data-urlencode "sound=pushover" \
    --data-urlencode "priority=0" \
    --data-urlencode "html=1" \
    --data-urlencode "title=Home Server" \
    --data-urlencode "message=$MSG" \
    https://api.pushover.net/1/messages.json
```

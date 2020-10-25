# Home Assistant

- [Homepage](https://www.home-assistant.io/)
- [Github repo](https://github.com/home-assistant/core)
- [DockerHub repo](https://hub.docker.com/r/homeassistant/home-assistant)
- [Docs](https://www.home-assistant.io/docs/)

## Links
- [awesome-ha](/Users/i313281/Projects/_playground/self-hosted-cookbook/apps/home-automation/home-assistant.md)
- [MDI icons](https://cdn.rawgit.com/james-fry/home-assistant-mdi/efd95d7a/home-assistant-mdi.html)
- [Full MDI icons](https://cdn.materialdesignicons.com/5.2.45/)
- [DubhAd/Home-AssistantConfig](https://github.com/DubhAd/Home-AssistantConfig)
- [compatible devices](https://www.hadevices.com/)

## Integrations
- [hacs](https://github.com/hacs/integration)
- [aarlo](https://github.com/twrecked/hass-aarlo)
- [garbage collection](https://github.com/bruxy70/Garbage-Collection)
- [unifi protect](https://github.com/briis/unifiprotect)
- [homeassistant-attributes](https://github.com/pilotak/homeassistant-attributes)

## Plugins
- [simple-thermostat](https://github.com/nervetattoo/simple-thermostat)
- [lovelace-hass-aarlo](https://github.com/twrecked/lovelace-hass-aarlo)
- [mini-graph-card](https://github.com/kalkih/mini-graph-card)
- [custom-header](https://github.com/maykar/custom-header)

## Themes
- [grey-night](https://github.com/home-assistant-community-themes/grey-night)
- [slate](https://github.com/seangreen2/slate_theme)


## docker-compose.yml
```yml
version: '3'
services:
  homeassistant:
    container_name: home-assistant
    image: homeassistant/home-assistant
    restart: unless-stopped
    environment:
      - TZ=Europe/Dublin
    ports:
      - 8123:8123
    volumes:
      - ./config:/config
```

## configuration.yml
```yml
default_config:

tts:  # Text to speech
  - platform: google_translate

group: !include groups.yaml
automation: !include automations.yaml
script: !include scripts.yaml
scene: !include scenes.yaml
frontend:
  themes: !include_dir_merge_named themes
homeassistant:
  customize: !include customize.yaml

updater:
  reporting: false

wake_on_lan:

aarlo:
  username: "email@example.com"
  password: "password"

webostv:
  host: <TV IP>
  name: LGTV
  turn_on_action:
    service: wake_on_lan.send_magic_packet
    data:
      mac: <MAC_ADDRESS>
      broadcast_address: <TV IP | SUBNET MASK>
      broadcast_port: 9
  customize:
    sources:
      - Apple TV
      - TV
      - YouTube
      - Plex

notify:
  - name: pushover_notifier
    platform: pushover
    api_key: <API KEY>
    user_key: <USER KEY>

calendar:
  - platform: caldav
    username: "<USERNAME>"
    password: "<PASSWORD>"
    url: <CALDAV URL>

camera:
  - platform: aarlo

media_player:
  - platform: aarlo

weather:
  - platform: gismeteo
    mode: daily
    latitude: 0
    longitude: 0

sensor:
  - platform: aarlo
    monitored_conditions:
      - recent_activity
      - captured_today
      - battery_level
  - platform: attributes
    friendly_name: "Batteries"
    attribute: battery_level
    unit_of_measurement: "%"
    entities:
      - sensor.some_sensor_light_level
```

## customize.yml
```yml
sensor.some_sensor_light_level_battery_level:
  friendly_name: Living Room

media_player.lgtv:
  source_list:
  - Apple TV
  - Plex
  - TV
  - YouTube
```

## Tips & Tricks

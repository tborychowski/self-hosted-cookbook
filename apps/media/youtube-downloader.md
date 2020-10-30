# YoutubeDL-web

- [Github repo](https://github.com/franhp/youtubedl-web)

![Screenshot](youtubedl-web.png)


## docker-compose.yml
```yml
---
version: '3'
services:
  youtubedl-web:
    image: franhp/youtubedl-web:latest
    container_name: youtubedl-web
    restart: unless-stopped
    ports:
      - "5000:5000"
    volumes:
      - ./downloads:/downloads
```

----

# YoutubeDL-Material

- [Github repo](https://github.com/Tzahi12345/YoutubeDL-Material)

![Screenshot](youtubedl-material.png)

## docker-compose.yml
```yml
---
version: "3"
services:
    ytdl_material:
        image: tzahi12345/youtubedl-material
        ports:
            - "8998:17442"
        restart: unless-stopped
        environment:
            ytdl_url: http://localhost:8998
            ytdl_port: '17442'
            ytdl_use_encryption: 'false'
            ytdl_audio_folder_path: video/
            ytdl_video_folder_path: video/
            ytdl_title_top: Youtube Downloader
            ytdl_allow_quality_select: 'true'
            ytdl_file_manager_enabled: 'false'
            ytdl_download_only_mode: 'false'
            ytdl_allow_multi_download_mode: 'true'
            ytdl_use_youtube_api: 'false'
            ytdl_youtube_api_key: 'false'
            ytdl_default_theme: dark
            ytdl_allow_theme_change: 'false'
            ytdl_use_default_downloading_agent: 'true'
            ytdl_custom_downloading_agent: 'false'
            ytdl_allow_advanced_download: 'false'
            write_ytdl_config: 'true'
            ALLOW_CONFIG_MUTATIONS: 'true'
        volumes:
			- ./downloads:/app/video
```

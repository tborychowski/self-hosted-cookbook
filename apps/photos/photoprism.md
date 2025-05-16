# Photoprism

Personal Photo Management powered by Go and Google TensorFlow.

<br>

- [Github repo](https://github.com/photoprism/photoprism)
- [Demo](https://demo.photoprism.org/photos)


## docker-compose.yml
```yml

services:
  photoprism:
    image: photoprism/photoprism:latest
    restart: unless-stopped
    ports:
      - 2342:2342
    shm_size: "2gb"
    depends_on:
      - photoprism-db
    environment:
      TF_CPP_MIN_LOG_LEVEL: 0
      PHOTOPRISM_SITE_URL: "http://localhost:2342/"
      PHOTOPRISM_SITE_TITLE: "PhotoPrism"
      PHOTOPRISM_SITE_CAPTION: "Browse Your Life"
      PHOTOPRISM_SITE_DESCRIPTION: "Open-Source Personal Photo Management"
      PHOTOPRISM_SITE_AUTHOR: "@browseyourlife"
      PHOTOPRISM_DEBUG: "false"
      PHOTOPRISM_READONLY: "false"
      PHOTOPRISM_PUBLIC: "true"
      PHOTOPRISM_EXPERIMENTAL: "true"
      PHOTOPRISM_UPLOAD_NSFW: "false"
      PHOTOPRISM_DETECT_NSFW: "false"
      PHOTOPRISM_SERVER_MODE: "production"
      PHOTOPRISM_HTTP_HOST: "0.0.0.0"
      PHOTOPRISM_HTTP_PORT: 2342
      PHOTOPRISM_DATABASE_DRIVER: "mysql"
      PHOTOPRISM_DATABASE_DSN: "root:photoprism@tcp(photoprism-db:4001)/photoprism?charset=utf8mb4,utf8&parseTime=true"
      PHOTOPRISM_TEST_DRIVER: "sqlite"
      PHOTOPRISM_TEST_DSN: ".test.db"
      PHOTOPRISM_ADMIN_PASSWORD: "photoprism"
      PHOTOPRISM_THUMB_FILTER: "lanczos"    # Resample filter, best to worst: blackman, lanczos, cubic, linear
      PHOTOPRISM_THUMB_UNCACHED: "true"     # Enable on-demand thumbnail rendering (high memory and cpu usage)
      PHOTOPRISM_THUMB_SIZE: 2048           # Pre-rendered thumbnail size limit (default 2048, min 720, max 7680)
      # PHOTOPRISM_THUMB_SIZE: 4096         # Retina 4K, DCI 4K (requires more storage); 7680 for 8K Ultra HD
      PHOTOPRISM_THUMB_SIZE_UNCACHED: 7680  # On-demand rendering size limit (default 7680, min 720, max 7680)
      PHOTOPRISM_JPEG_SIZE: 7680            # Size limit for converted image files in pixels (720-30000)
      PHOTOPRISM_JPEG_QUALITY: 92           # Set to 95 for high-quality thumbnails (25-100)
      PHOTOPRISM_DARKTABLE_PRESETS: "false" # Use darktable presets (disables concurrent raw to jpeg conversion)
      PHOTOPRISM_SIDECAR_JSON: "true"       # Read metadata from JSON sidecar files created by exiftool
      PHOTOPRISM_SIDECAR_YAML: "true"       # Backup photo metadata to YAML sidecar files
      PHOTOPRISM_ASSETS_PATH: "/photoprism/assets"
      PHOTOPRISM_ORIGINALS_PATH: "/photoprism/storage/originals"
      PHOTOPRISM_IMPORT_PATH: "/photoprism/storage/import"
      PHOTOPRISM_STORAGE_PATH: "/photoprism/storage/cache" # Storage PATH for generated files like cache and index
    volumes:
      - "./storage:/photoprism/storage"     # Keep cache, settings and database

  photoprism-db:
    image: mariadb:10.5
    restart: unless-stopped
    command: mysqld --port=4001 --transaction-isolation=READ-COMMITTED --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --max-connections=512 --innodb-rollback-on-timeout=OFF --innodb-lock-wait-timeout=50
    volumes:
      - "./db:/var/lib/mysql"
    environment:
      MYSQL_ROOT_PASSWORD: photoprism
      MYSQL_USER: photoprism
      MYSQL_PASSWORD: photoprism
      MYSQL_DATABASE: photoprism

```

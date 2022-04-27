# Notea

- This is probably more of a note-taking app rather than a wiki, but why not :-)
- Setup with docker is a bit more challenging than the usual `docker-compose up -d`
- Layout is very minimalistic
- Command palette!
- `/<command>` when writing is great!
- Extremely fast to start-up (not really a feature, but still impressive)

<br>

- [Homepage](https://cinwell.com/notea/)
- [GitHub repo](https://github.com/QingWei-Li/notea)


## docker-compose.yml
```yml
---
services:
  notea:
    image: cinwell/notea
    container_name: notea
    environment:
      - STORE_ACCESS_KEY=086o0ITYnydXRzxO
      - STORE_SECRET_KEY=7FKZabUq7LCWyhgYyTh39A26vZMKb99G
      - STORE_BUCKET=notea
      - STORE_END_POINT=http://notea-db:9000
      - STORE_FORCE_PATH_STYLE=true
      - COOKIE_SECURE=false
      - BASE_URL="http://192.168.1.10:3123/"
      #- PASSWORD=notea
      - DISABLE_PASSWORD=true
    ports:
      - "3123:3000"

  notea-db:
    image: minio/minio
    container_name: notea-db
    command: server /data  --console-address ":9001"
    environment:
      #- MINIO_BROWSER=off
      - MINIO_ROOT_USER=admin
      - MINIO_ROOT_PASSWORD=admin123
    ports:
      - "3124:9000"
      - "3125:9001"
    volumes:
      - ./data:/data
```

### Next steps
Notea requires external storage. Minio container stands in place of Amazon S3. We just need to create a "Bucket" in minio, so:
#### Create a Bucket
1. Go to <serverIP:3125> (- minio admin console)
2. Login using `admin:admin123` credentials from `docker-compose.yml` file
3. Create a new bucket with the name `notea` (no need for any other options, just name)

#### Access Keys
1. Next click "Identity/Service Accounts" in the sidebar and click "Create service account" button
2. Either copy the "Access Key" and "Secret Key" from the yaml above or generate a new pair of keys and update them in your `docker-compose.yml` (and restart it afterwards).

That's it!
Notea should be available at <serverIP:3123>

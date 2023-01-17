# Traccar

Traccar is a gps tracking system. You can use it to monitor your smartphone location history

<br>

- [website](https://www.traccar.org/)
- [documentation](https://www.traccar.org/documentation/)

## get the default traccar conf

```sh
docker run --rm --entrypoint cat debian-traccar-nginx:latest /opt/traccar/conf/traccar.xml > /run/media/ippo/TOSHIBA/traccar/conf/traccar.xml
```

## Configuration parameters for MySQL
Replace [DATABASE], [USER], [PASSWORD] with appropriate values from `docker-compose.yml` replace `[HOST]` with IPv4 address from `db_name` section in `docker network inspect`:

```xml
<entry key='database.driver'>com.mysql.cj.jdbc.Driver</entry>
<entry key='database.url'>jdbc:mysql://[HOST]:3306/[DATABASE]?serverTimezone=UTC&amp;allowPublicKeyRetrieval=true&amp;useSSL=false&amp;allowMultiQueries=true&amp;autoReconnect=true&amp;useUnicode=yes&amp;characterEncoding=UTF-8&amp;sessionVariables=sql_mode=''</entry>
<entry key='database.user'>[USER]</entry>
<entry key='database.password'>[PASSWORD]</entry>
```

## docker-compose

```yaml
---
services:
    traccar-db:
        image: yobasystems/alpine-mariadb
        container_name: traccar-db
        restart: unless-stopped
        command: --default-authentication-plugin=mysql_native_password
        networks:
            - trc2
        environment:
            - MYSQL_ROOT_PASSWORD=rootpassword
            - MYSQL_DATABASE=traccar-db
            - MYSQL_USER=username
            - MYSQL_PASSWORD=userpassword
        volumes:
            - ./mysql-data:/var/lib/mysql
            - ./mysql:/etc/mysql/conf.d
        ports:
            - "3306:3306"

    traccar:
        image: traccar/traccar:debian
        hostname: <server IP>
        container_name: traccar
        restart: unless-stopped
        depends_on:
           - traccar-db
        networks:
            - trc2
        ports:
            - "5055:5055"
            - "82:8082"
        volumes:
            - ./traccar.xml:/opt/traccar/conf/traccar.xml:ro
            - ./logs:/opt/traccar/logs:rw

networks:
  trc2:
    driver: bridge
    enable_ipv6: false
    ipam:
      config:
        # get the network subnet: `docker network inspect network_name`
        - subnet: 192.168.112.0/20
```

## open root mysql shell create database, charset and priviledget user

```sh
docker exec -it traccar-db mysql -u root -p
```

```sql
CREATE DATABASE IF NOT EXISTS traccar-db
grant all privileges on traccar-db.* TO user'@'% identified by pass
flush privileges
ALTER DATABASE traccar CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ciALTER DATABASE traccar CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci
\q
```


## Tips & tricks

### Backup traccar mysql

```sh
docker compose -f  compose-file.yml exec dbname mysqldump -uroot -pYOUR_MARIADB_ROOT_PASSWORD --all-databases > dump-$(date +%F_%H-%M-%S).sql
```

### Restore traccar mysql

```
docker compose -f compose-file.yml exec -T dbname mysql -uroot -pYOUR_MARIADB_ROOT_PASSWORD < mariadb-dump.sql
```

### Android apps

- [Android client app](https://www.traccar.org/client/)
On client app While service is deactivated Copy the identifier, Insert the server URL (your dens,domain), Select location accuracy (high), Select frequency report ~120 sec, disable wakelock  and enable the service

On status verify that the location is updating

- [Android manager app](https://www.traccar.org/manager/)
On server Top left menu Select the plus icon to add a device Select a random name And the identifier from the client app And save

The you can select the device on the top left menu


### Migrate google location history takeout to traccar

[download your google maps location history takeout](https://takeout.google.com/takeout/custom/local_actions,location_history,maps,mymaps?)

- select location history only
- and json as format
- extract the Records.json

you'll use a python script to limit the resaults to just time , latitude and longitude converted to proper coordinates

```sh
git clone https://github.com/Scarygami/location-history-json-converter
cd location-history-json-converter
pip install -r requirements.txt
python location_history_json_converter.py Records.json  output.csv -f csv
```

csv output will generate Comma-separated text file with a timestamp field and a location field
we'll add a deviceid , protocol and valid fileds to it

```sh
sed 's/^/osmand,1,/; s/$/,1/' export.csv > curated.csv
```

delete the "1" from the first line an replace osmand with protocol in the first line, so now the file looks like

```csv
protocol,Time,Latitude,Longitude
osmand,1,2012-08-25 21:26:20,37.95954620,23.72793730,1
```

5 columns
osmand
1
time
lat
lon

We are going to parse those values to as sql `LOAD DATA LOCAL INFILE` statement to the appropriate tc_position table fields

- copy the csv to the container
    ```sh
    docker cp curated.csv traccar-db:/
    ```

- open a root mysql shell to the db container
    ```sh
    docker exec -it traccar-db mysql -uroot -pROOTPASSWORD
    ```

- connect to database
    ```sh
    use traccar-db
    ```

- load the data:
    ```sql
    LOAD DATA LOCAL INFILE 'inn.csv' INTO TABLE tc_positions FIELDS TERMINATED BY ',' (@osmand,    @deviceid,@Time,@Latitude,@Longitude,@valid) set protocol=@osmand,deviceid=@deviceid,    devicetime=@Time,fixtime=@Time,servertime=@Time,latitude=@Latitude,longitude=@Longitude,    valid=@valid;
    ```

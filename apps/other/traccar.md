# Traccar

![traccar_hu4f4d748acb69c0f52e2340aac180cda5_35275_1600x0_resize_box_3](https://user-images.githubusercontent.com/52239579/212997283-e3279b61-189f-452c-86e5-d1e2fe216da2.png)

**Traccar is a gps tracking system. You can use it to monitor your smartphone location history**

<br>

[website](https://www.traccar.org/)

[documentation](https://www.traccar.org/documentation/)

## get the default traccar conf 

`docker run --rm --entrypoint cat debian-traccar-nginx:latest /opt/traccar/conf/traccar.xml > /run/media/ippo/TOSHIBA/traccar/conf/traccar.xml`

## Configuration parameters for MySQL 
(replace [DATABASE], [USER], [PASSWORD] with appropriate values from docker-compose replace [HOST "IPv4Address" from db_name section in docker network inspect)

```
<entry key='database.driver'>com.mysql.cj.jdbc.Driver</entry>
<entry key='database.url'>jdbc:mysql://[HOST]:3306/[DATABASE]?serverTimezone=UTC&amp;allowPublicKeyRetrieval=true&amp;useSSL=false&amp;allowMultiQueries=true&amp;autoReconnect=true&amp;useUnicode=yes&amp;characterEncoding=UTF-8&amp;sessionVariables=sql_mode=''</entry>
<entry key='database.user'>[USER]</entry>
<entry key='database.password'>[PASSWORD]</entry>
```

## docker-compose

```
version: "3"

services:
    traccar-db:
#yobasystems was used for its compact size. also it survives recomposing and does not throws innodb error when volumes are not empty as the official mariadb image does    
        image: yobasystems/alpine-mariadb
        container_name: traccar-db
#auth plugin for old drivers        
        command: --default-authentication-plugin=mysql_native_password
        restart: always
        volumes:
# mount /var/lib/mysql and /etc/mysql/conf.d on host        
            - /run/media/ippo/TOSHIBA/traccar/mysql-data:/var/lib/mysql
            - /run/media/ippo/TOSHIBA/traccar/mysql:/etc/mysql/conf.d
        ports:
            - "3306:3306"
        environment:
#use those in the traccar config file in /opt/traccar/conf/traccar.xml as mounted in host  
            - MYSQL_ROOT_PASSWORD=rootpassword
            - MYSQL_DATABASE=traccar-db
            - MYSQL_USER=username
            - MYSQL_PASSWORD=userpassword
        networks:
            - trc2


    traccar:
        image: traccar/traccar:debian
#create an account to a dynamic dns provider e.g. duckdns then reverse proxy it to localhost:traccar_port e.g. with caddy.  Caddyfile is dead simple    
        hostname: a_ddns_domain_name
        container_name: traccar
        depends_on:
           - traccar-db
        restart: always
        volumes:
#get the default traccar conf 
#then replace the embeded h2 db conf with "mysql" conf
#https://www.traccar.org/mysql/
#get the network subnet and ip 
#docker network inspect network_name  in the database section
#use the ip in the [hostname] section of the database url database.url'>jdbc:mysql://[HOST]/[DATABASE]?
            - /run/media/ippo/TOSHIBA/traccar/conf/traccar.xml:/opt/traccar/conf/traccar.xml:ro
            - /run/media/ippo/TOSHIBA/traccar/logs:/opt/traccar/logs:rw
        ports:
#android client needs tcp 5055 port so if you dont plan to use any of the supported hardware gps trackers
#you can skip listening to 5002,5093 tcp/udp ports and only keep 5055 for events and 8082 for the webui      
            - "5055:5055"
            - "82:8082"
        networks:
            - trc2
networks:
  trc2:
    driver: bridge
    enable_ipv6: false
    ipam:
      config:
#get the network subnet
#docker network inspect network_name      
        - subnet: 192.168.112.0/20
```

## open root mysql shell create database, charset and priviledget user

`docker exec -it traccar-db mysql -u root -p`
`CREATE DATABASE IF NOT EXISTS traccar-db`
`grant all privileges on traccar-db.* TO user'@'% identified by pass`
`flush privileges`
`ALTER DATABASE traccar CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ciALTER DATABASE traccar CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci`
`\q`

## reverse proxy

i recoment to use caddy for reverse proxy and automatic ssl manager

`nano /etc/caddy/Caddyfile`

`a.random.ddns.name.duckdns.org {
    reverse_proxy localhost:82
}`

## backup and restore traccar mysql

backup

`docker-compose -f  compose-file.yml exec dbname mysqldump -uroot -pYOUR_MARIADB_ROOT_PASSWORD --all-databases > dump-$(date +%F_%H-%M-%S).sql`

restore

`docker-compose -f compose-file.yml exec -T dbname mysql -uroot -pYOUR_MARIADB_ROOT_PASSWORD < mariadb-dump.sql`

## android apps

[Android client app](https://www.traccar.org/client/)

On client app While service is deactivated Copy the identifier, Insert the server URL (your dens,domain), Select location accuracy (high), Select frequency report ~120 sec, disable wakelock  and enable the service

On status verify that the location is updating

[Android manager app](https://www.traccar.org/manager/)

On server Top left menu Select the plus icon to add a device Select a random name And the identifier from the client app And save

The you can select the device on the top left menu

![Screenshot_20230117-214656_Traccar Manager](https://user-images.githubusercontent.com/52239579/212997717-452c1e22-b244-4f50-8806-a61daa2a9485.png)


## Migrate google location history takeout to traccar

[download your google maps location history takeout](https://takeout.google.com/takeout/custom/local_actions,location_history,maps,mymaps?)

select location history only
and json as format
extract the Records.json

you'll use a python script to limit the resaults to just time , latitude and longitude converted to proper coordinates

`git clone https://github.com/Scarygami/location-history-json-converter`

`cd location-history-json-converter`

`pip install -r requirements.txt`

`python location_history_json_converter.py Records.json  output.csv -f csv`

csv output will generate Comma-separated text file with a timestamp field and a location field
we'll add a deviceid , protocol and valid fileds to it

`sed 's/^/osmand,1,/; s/$/,1/' export.csv > curated.csv`

delete the "1" from the first line an replace osmand with protocol in the first line

so now the file looks like

```
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

copy the csv to the container

`docker cp curated.csv traccar-db:/`

open a root mysql shel to the db container

`docker exec -it traccar-db mysql -uroot -pROOTPASSWORD`

connect to database

`use traccar-db`

load the data 

```
LOAD DATA LOCAL INFILE 'inn.csv' INTO TABLE tc_positions FIELDS TERMINATED BY ',' (@osmand,@deviceid,@Time,@Latitude,@Longitude,@valid) set protocol=@osmand,deviceid=@deviceid,devicetime=@Time,fixtime=@Time,servertime=@Time,latitude=@Latitude,longitude=@Longitude,valid=@valid;
```

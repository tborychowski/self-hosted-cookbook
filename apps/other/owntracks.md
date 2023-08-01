# owntracks

- Owntracks is a location history tracking app
- Mosquitto is a mqtt protocol,  publish/subscribe message  brocker that stores data that recieves from clients
-  Recorder is a lightweight program for storing and accessing location data published via MQTT (or HTTP) and displays the in a web ui on a map as tracks,  points etc
- The android app tracks location and sends the location data to mosquitto, then owntracks recorder get the data from mosquitto and graphically displays them in a webui
- mTLS is used to authenticate clients, whereas "normal" TLS just authenticates the server. The authentication is now mutual!
- mTLS is one of the puzzle pieces of building a Zero Trust Network as it strictly controls which clients are allowed to connect to a service regardless of where a user or device is connecting from 
- in the below setup the android app connects with mtls with mosquitto and the browser connects with mtls with the webui through caddy
- Caddy 2 is a powerful, enterprise-ready, open source web server with automatic HTTPS written in Go
- Recorder also supports tls but I suffered trying to make it work without success as it talks with mosquitto only inside our lan and is basic auth protected I'm still fine with the current setup.

<br>

- [github repo](https://github.com/owntracks/docker-recorder)

- [client certs](https://owntracks.org/booklet/features/tlscert/)

- [Mosquitto tls](http://www.steves-internet-guide.com/mosquitto-tls/)

- [Mosquitto tls 2](https://medium.com/himinds/mqtt-broker-with-secure-tls-and-docker-compose-708a6f483c92)

- [caddy mtls](https://www.reddit.com/r/selfhosted/comments/shvkkb/reverse_proxy_client_certificates_for_dummies/)

## certs.sh
```
#!/bin/bash

IP="your_lan_ip"
SUBJECT_CA="/C=SE/ST=Athens/L=Athens/O=ippo/OU=CA/CN=$IP"
SUBJECT_SERVER="/C=SE/ST=Athens/L=Athens/O=ippo/OU=Server/CN=$IP"
SUBJECT_CLIENT="/C=SE/ST=Athens/L=Athens/O=ippo/OU=Client/CN=$IP"

function generate_CA () {
   echo "$SUBJECT_CA"
   openssl req -x509 -nodes -sha256 -newkey rsa:2048 -subj "$SUBJECT_CA"  -days 3650 -keyout ca.key -out ca.crt
}

function generate_server () {
   echo "$SUBJECT_SERVER"
   openssl req -nodes -sha256 -new -subj "$SUBJECT_SERVER" -keyout server.key -out server.csr
   openssl x509 -req -sha256 -in server.csr -CA ca.crt -extfile v3.ext -CAkey ca.key -CAcreateserial -out server.crt -days 3650
}

function generate_client () {
   echo "$SUBJECT_CLIENT"
   openssl req -new -nodes -sha256 -subj "$SUBJECT_CLIENT" -out client.csr -keyout client.key 
   openssl x509 -req -sha256 -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 3650
}

generate_CA
generate_server
generate_client
```

## v3.ext
```
subjectKeyIdentifier   = hash

authorityKeyIdentifier = keyid:always,issuer:always

basicConstraints       = CA:TRUE

keyUsage               = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment, keyAgreement, keyCertSign

subjectAltName         = DNS:mqtt.example.org, DNS:localhost

issuerAltName          = issuer:copy


```

## PKCS#12 cert
`openssl pkcs12 -export -out cert.p12 -inkey client.key -in client.crt -legacy`

## config/recorder.conf
```
OTR_TOPICS = "owntracks/#"
OTR_HTTPHOST = "0.0.0.0"
OTR_HOST = "your lan ip"
OTR_USER = "user"
OTR_PASS = "pass"
```

## mosquitto/config/mosquitto.conf
```
persistence true
persistence_location /mosquitto/data/
listener 1883
password_file /mosquitto/passwd/pass

listener 8883

cafile /mosquitto/config/ca.crt
certfile /mosquitto/config/server.crt
keyfile /mosquitto/config/server.key
require_certificate true
use_identity_as_username true
protocol websockets
```
docker-compose.yml
```yml
---
services:
    mosquitto:
        image: eclipse-mosquitto:openssl
        container_name: mosquitto
        restart: unless-stopped
        ports:
            - "1883:1883"
            - "8883:8883"
        volumes:
            - "./mosquitto/config:/mosquitto/config"
            - "./mosquitto/data:/mosquitto/data"
            - "./mosquitto/config/passwd:/mosquitto/passwd"
        environment:
            - TZ=Europe/Athens
        user: "1000:1000"
    otrecorder:
        image: ot-arm:latest
        ports:
            - 8083:8083
        volumes:
            - ./config:/config
            - ./store:/store
        restart: unless-stopped
```

## Dockerfile
```
FROM alpine:3.16 AS builder

ARG RECORDER_VERSION=0.9.3
# ARG RECORDER_VERSION=master

RUN apk add --no-cache \
        make \
        gcc \
        git \
        shadow \
        musl-dev \
        curl-dev \
        libconfig-dev \
        mosquitto-dev \
        lmdb-dev \
        libsodium-dev \
        lua5.2-dev \
 util-linux-dev

RUN git clone --branch=${RECORDER_VERSION} https://github.com/owntracks/recorder /src/recorder
WORKDIR /src/recorder

COPY config.mk .
RUN make -j $(nprocs)
RUN make install DESTDIR=/app

FROM alpine:3.16

VOLUME ["/store", "/config"]

RUN apk add --no-cache \
 curl \
 jq \
 libcurl \
 libconfig \
 mosquitto \
 lmdb \
 libsodium \
 lua5.2 \
 util-linux

COPY recorder.conf /config/recorder.conf
COPY JSON.lua /config/JSON.lua
COPY --from=builder /app /

COPY recorder-health.sh /usr/sbin/recorder-health.sh
COPY entrypoint.sh /usr/sbin/entrypoint.sh

RUN chmod +x /usr/sbin/*.sh
RUN chmod +r /config/recorder.conf

EXPOSE 8083

# ENV OTR_CAFILE=/etc/ssl/cert.pem
ENV OTR_STORAGEDIR=/store
ENV OTR_TOPIC="owntracks/#"

ENTRYPOINT ["/usr/sbin/entrypoint.sh"]
```

## basic auth
`docker exec -it --user root mosquitto mosquitto_passwd -c /mosquitto/passwd/pass username`


## android app settings
```
Connection:

mode mqtt

host mqtt.example.org

Port 8883 (open port on router)

Client ID random name

Websockets toggle enabled

Identification:

Username random name (will be displayed on recorder ui)

Password empty

Device ID random name

Tracker ID random name

Security:

TLS enabled

select client cert under preferences>connection>security

CA cert empty (installed the ca.crt in the device user store)
```

## Caddy certs

Request a new key and crt
`openssl req -x509 -newkey rsa:4096 -keyout cert_name.key -out cert_name.crt -days 365`

Request a new certificate signing request
`openssl req -new -key cert_name.key -out cert_name.CSR`

Request a new certificate authority
`openssl x509 -req -days 365 -in cert_name.csr -signkey cert_name.key -out cert_name-CA.crt`

Create a pem certificate
`cat cert_name.crt cert_name.key > cert_name.pem`

Create a pkcs12 certificate
`openssl pkcs12 -export -out cert_name.p12 -inkey cert_name.key -in cert_name.pem -legacy`

## /etc/caddy/Caddyfile
```
#cert directive
(fancy_name) {
  tls {
    client_auth {
      mode require_and_verify
      trusted_ca_cert_file /var/lib/caddy/cert/cert_name-CA.crt
      trusted_leaf_cert_file /var/lib/caddy/cert/cert_name.crt
    }
  }
}

#owntracks
owntracks.example.org {
import fancy_name
reverse_proxy localhost:8083
}
```

## Folder structure

```
ls -R
.:
config  docker-compose.yml  mosquitto  store
./config:
recorder.conf
./mosquitto:
config  data
./mosquitto/config:
ca.crt  mosquitto.conf  passwd  server.crt  server.key
./mosquitto/config/passwd:
pass
./mosquitto/data:
./store:
ghash  last  monitor  rec
./store/ghash:
data.mdb  lock.mdb
```

## Tips & Tricks

#### create the certificates for mosquitto and owntracks android app

Change IP and ST,L,O for ca,server,client crt's also pick an appropriate -days for them in certs.sh

Copy ca.crt,server.crt,server.key to mosquitto/config

#### v3.ext  file for filling S.A.N. field

Note subjectAltName use your dynamic dns address
This is mantatory this will be the allowed domain for this certificate

Place ext file on the same dir with the script

#### pkcs12 bundle

Transfer ca.crt and cert.p12 on android device

Install ca.crt on android>settings>security>encryption>install a certificate>ca certificate

Select cert.p12 under owntracks>preferences>connection>security

Android can't  handle modern pkcs encryption algorythms (PBES2, PBKDF2, AES-256-CBC, Iteration 2048, PRF hmacWithSHA256) that is used on openssl v3 . You can omit the -legacy flag if you are creating the pkcs cert with older openssl versions

#### directories

`mkdir {config,mosquitto,store,store/last}`

Before everything else create the needed directories

#### Dockerfile

Owntracks recorder publishes x86 images on dockerhub but there are no official ARM images so you have to build if you are on arm

Mosquitto publishes images for all aarch's

#### Basic auth

Comment password_file on mosquitto/config/mosquitto.conf on first run then run mosquitto_passwd command as user root on the mosquitto container and finaly uncomment password_file and re-run docker compose up

#### Caddy certs

On android you have to install the cert_name.p12 cert as vpn & app user certificate under settings > security > more > credentials  > install > VPN & app user cert

Copy cert_name-CA.crt and cert_name.crt to /var/lib/caddy/cert/

#### domains

We used two domains
One for publishing mqtt location messages from android to mosquitto (mqtt.example.org)
And one for accessing the recorder webui with our browser (owntracks.e.ample.org)

#### certificates

We installed two certificates on the android certificate store
The certificate authority for the mosquitto client cert ca.crt 
The caddy client certificate cert_name.p12
We selected the mosquitto client cert from within the owntracks app cert.p12
We created two certificate authorities. The caddy directive "fancy_name" can be imported for other services that you reverse proxy with caddy also


#### Launch

You can view your location history visiting owntracks.example.org
If more than one device connects to the same broker (mqtt.example.org ) you can also view each others current location on the android app and the history on ot-recorder (owntracks.example.org)

# Get started with docker & docker-compose

Installation instructions for Ubuntu Linux from: https://docs.docker.com/engine/install/ubuntu/


## Uninstall old versions
```sh
sudo apt-get remove docker docker-engine docker.io containerd runc
```

## Install Docker
```sh
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```


## Install `docker-compose`
From: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04

```sh
sudo apt-get remove docker-compose
sudo curl -L "https://github.com/docker/compose/releases/tag/1.29.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```


## Usage
Full docs are here: https://docs.docker.com/compose/reference/overview/

Generally the idea of `docker-compose` command is simple:
1. First you create a folder for your service, e.g. `home-assistant`
2. Then you then create a `docker-compose.yml` file, which describes the service containers
3. You run `docker-compose up -d` and you're done!

Here are the most frequently used commands:


#### Start a container
`-d` starts in a detached mode - which basically mean that it runs it in the background (so you can do other stuff, instead of looking at the logs).
```sh
docker-compose up -d
```

### Stop a container
There are 2 options:
```sh
docker-compose stop
docker-compose down
```
First one (`stop`) onlu stops the containers, whereas the latter (`down`) does some more cleaning (removes the containers, networks, volumes), so I generally recommend `down` if you're tinkering.


### Update a container to use the latest published image
```sh
docker-compose pull && docker-compose up -d
```
This pulls the latest images as defined in the local `docker-compose.yml` and recreates the containers.

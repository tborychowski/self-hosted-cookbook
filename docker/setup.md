# Docker Setup

Installation instructions for Ubuntu Linux: https://docs.docker.com/engine/install/ubuntu/


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
### ERROR: Version in "./docker-compose.yml" is unsupported.
```sh
sudo apt-get remove docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

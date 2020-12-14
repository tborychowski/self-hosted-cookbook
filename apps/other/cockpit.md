# Cockpit


- [Github repo](https://github.com/cockpit-project/cockpit)
- [Docs](https://cockpit-project.org/running.html#ubuntu)


## Installation
```sh
sudo apt-get install cockpit
```

Then go to `https://<SERVER IP>:9090`


## Tips & Tricks
To remove the cockpit's motd (welcome message) automatically added to every ssh login:
```sh
sudo rm /etc/motd.d/cockpit
```

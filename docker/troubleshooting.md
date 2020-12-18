# Docker Troubleshooting
Same as **Get Started** section - these instructions are for Ubuntu Linux.

### ERROR: Version in "./docker-compose.yml" is unsupported.
You need to update your `docker-compose` version:
```sh
sudo apt-get remove docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```


### ERROR: Couldn't connect to Docker daemon at http+docker://localhost - is it running?
```sh
sudo reboot
```


### ERROR: Permission denied while trying to connect to the Docker daemon
```sh
sudo usermod -a -G docker $USER
```


### ERROR: mountpoint for devices not found
e.g.
```
OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:297: applying cgroup configuration for process caused \"mountpoint for devices not found\"": unknown
```

#### Follow These Steps:
1. Edit grub
  ```sh
  sudo nano /etc/default/grub
  ```
2. Change `GRUB_CMDLINE_LINUX` value as below:
  ```
  GRUB_CMDLINE_LINUX="systemd.unified_cgroup_hierarchy=0"
  ```
3. Reconfigure grub:
  ```sh
  grub2-mkconfig
  ```
4. Finally reboot the system:
  ```sh
  sudo reboot
  ```


### INTERNAL ERROR: cannot create temporary directory
From: [stackoverflow](https://stackoverflow.com/questions/40755494/docker-compose-internal-error-cannot-create-temporary-directory)
Looks like the docker host run out of free space.
Free up some space on your hard disk and reboot.
Sometimes that may happen due to a crazy docker service running wild.
You can try this to prune everything docker:
```sh
docker system prune -a --volumes
```

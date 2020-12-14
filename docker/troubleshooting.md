# Docker Troubleshooting


### ERROR: Version in "./docker-compose.yml" is unsupported.
```sh
sudo apt-get remove docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
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
1. edit grub
  ```sh
  sudo nano /etc/default/grub
  ```
2. change GRUB_CMDLINE_LINUX value to the below:
  ```
  GRUB_CMDLINE_LINUX="systemd.unified_cgroup_hierarchy=0"
  ```
3. then run:
  ```sh
  grub2-mkconfig
  ```
4. then:
  ```sh
  sudo reboot
  ```

# Setup Docker (Ubuntu 18.04)
## Install Docker
Open a terminal and, from your user *$home*, run:
```sh
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
sudo apt install docker-ce
```

Docker should now be installed, the daemon started, and the process enabled to start on boot.

Check that it’s running:
```
sudo systemctl status docker
````

The output should be similar to the following, showing that the service is active and running:
```
docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2019-09-26 11:54:28 PDT; 12h ago
     Docs: https://docs.docker.com
 Main PID: 1000 (dockerd)
    Tasks: 39
...
```
## Executing the Docker Command Without Sudo
Add your username to the docker group:
```bash
sudo usermod -aG docker ${USER}
su - ${USER}
```
## Install Docker Compose
```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-Linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Then we’ll verify that the installation was successful by checking the version:
```
docker-compose --version
```

This will print out the version we installed:
```
docker-compose version 1.24.1, build 4667896b
```
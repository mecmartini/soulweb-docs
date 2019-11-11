# Setup Docker (MacOS)
## Install Docker Desktop for Mac
The Docker Desktop installation includes [Docker Engine](https://docs.docker.com/engine/userguide/), Docker CLI client, [Docker Compose](https://docs.docker.com/compose/), [Docker Machine](https://docs.docker.com/compose/), and [Kitematic](https://docs.docker.com/kitematic/userguide/).

1. Download [Docker Desktop](https://download.docker.com/mac/stable/Docker.dmg) for `Mac`
2. Double-click Docker.dmg to open the installer, then drag the Docker icon to the Applications folder

    ![Docker Desktop Installer](../../img/docker/docker-app-drag.png "Docker Desktop Installer")
    
3. Double-click Docker.app in the Applications folder to start Docker

    ![Start Docker Desktop](../../img/docker/docker-app-in-apps.png "Start Desktop Installer")

You are prompted to authorize Docker.app with your system password after you launch it. Privileged access is needed to install networking components and links to the Docker apps.

The Docker menu in the top status bar indicates that Docker Desktop is running, and accessible from a terminal.

![Start Docker Desktop](../../img/docker/whale-in-menu-bar.png "Start Desktop Installer")
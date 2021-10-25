___

## Install prerequisites

For now, this project has been mainly created for Unix `(Linux/MacOS)`. Perhaps it could work on Windows.

All requisites should be available for your distribution. The most important are :

* [Git](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/engine/installation/)
* [Docker Compose](https://docs.docker.com/compose/install/)

Check if `docker-compose` is already installed by entering the following command : 

```sh
which docker-compose
```

Check Docker Compose compatibility :

* [Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)


## Installation
- Clone or download the project
- Change directory to the specified directory (default to `enpii-dev-suite`)
- With Linux, you can use command line without `sudo` for docker, by:
```sh
# Create the docker group.
sudo groupadd docker

# Add your user to the docker group.
sudo usermod -aG docker ${USER}

# Login and verify that can run docker commands without sudo.
su -l ${USER}
``` 
Go to installation:

```sh
cd <path/to/project-directory>
# Init the .env file, then you can repair value needed to be updated, or you can copy `.env.example` -> `.env`
./scripts/init-env-file.sh 

# Init dev suite, you can add first option for the namespace of dev suite to overwrite the one in .env file
./scripts/init-dev-suite.sh
```
`chmod +x /scripts/init-dev-suite.sh` if you can't execute it
For Mac user, it's better to create it inside your home, e.g `<path/to/project-directory>` = `~/workspace/enpii-dev-suite/`
- Repair params on `docker-compose.yml` of `etc/*.conf` or `etc/*.ini` files to match your local
- Run Docker Compose
```sh
docker-compose up -d
```
- Wait for several mins and you'll have:
  - Nginx (work as a webserver)
  - PHP-FPM (PHP execution server)
  - MySql
  - phpmyadmin (should connect to mysql via host host.docker.internal)
  - We include MySQL in docker containers but because we believe database is important and you may lose you db once docker failed. Using a database server on local machine is out proposal: use `host.docker.internal` (for mac), `10.0.2.2` (for docker machine) for the hostname to connect to your main machine.

___

## Troubleshooting

### 1. Cannot start service nginx_main

If you have ERROR that cannot start service nginx_main. Because apache2 (or other service , you can find) use same port with it 
You must stop that service by
```sh
sudo service apache2 stop
```
And rerun by:
```sh
docker-compose up -d
```
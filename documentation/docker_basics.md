# Docker Basics

This file contains some docker notions you may need to complete the exercice.
<br>
Don't hesiate to have a look here: [full docker documentation](https://docs.docker.com)

## Install docker

## Runing a docker container

To run a docker container the `docker run` command is formatted as follow:
```shell
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

### Exposing docker container port

To map a container port to one of your machine do as follow:
```shell
docker run -d -p8080:3306 mysql
```

In this example we run a docker container containing the `latest` image of `mysql` with the dafault port of `mysql 3306` mapped to the port `8080` of your machine.
<br>
The `-d` option tells docker to run in dettach mode.

## Docker-compose
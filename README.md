# Infrastructure

This is a wild project that spins up most of my websites with docker-compose.
Pre-requisits for running this is having docker and docker-composed installed.
Additionally having mysql-client-default installed will be helpful for managing sql databases
within the docker images.

All of the websites contributing towards this have their own docker files in their respective github projects, minus my gitea instance.
The glory of this project is that it manages all of the let's encrypt certs for you and places them in the volumes/proxy/certs directory and then links it to the nginx container to use.


## Cloning the project:

```bash
git clone https://github.com/jrtechs/Infrastructure.git
git submodule init
git submodule update --remote Club-Panda
```


Notes on how to create a submodule. After this command you will have to change the url to point towards github in the .gitmodules directory.

```bash
git submodule add ./Club-Panda/.git
```

## Running Docker Compose

You only need to create the nginx-proxy network bridge once.

```bash
docker network create -d bridge nginx-proxy
```

Regular running:

```bash
docker-compose build
docker-compose up
```


## Configuration

Docker-compose.yml has most of the configurations in terms of host names and ports.
However, there are still manual configurations for each sub-project.
Sql.env file will contain root mysql password.
I am using root as a SQL user for convenience and because every application has its own database server.
Password configurations are found in the sql.env file. (not committed to git)


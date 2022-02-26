# Infrastructure

This is a wild project that spins up most of my websites with docker-compose.
Pre-requisits for running this is having docker and docker-composed installed.
Additionally having mysql-client-default installed will be helpful for managing sql databases
within the docker images.

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


## Nginx Configuration

The base server is configured with a nginx server that does reverse proxies to all of the services
that run in docker containers.
This gives me http encryption for all my web servers with let's encrypt.

```
# nginx installation

apt install nginx

# has nginx start on startup
systemctl enable nginx
```

Boilerplate nginx configration a simple site in the the /etc/nginx/sites-enabled/ folder:

```
server
{
    server_name jrtechs.net;

    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    location / {
        proxy_pass http://localhost:8000;
        proxy_redirect off;
        proxy_set_header Host $http_host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    listen 80;
    listen [::]:80;
}
```

You can use this command to test the syntax of the nginx config. 

```
nginx -t
```


```
# insall let's encrypt
apt install python3-certbot-nginx

# initial registration
certbot --nginx -d jrtechs.net


# placed in cron job for certification renewal
certbot certonly --standalone -d jrtechs.net
```

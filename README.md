## Verified to work in Release
To make this project work in Release, we had to add a COPY command to the Dockerfile in app/Dockerfile to copy index.php into the container. When run locally, docker-compose creates an overlay filesystem and uses files in the directory, but when run in a hosted environment we need to be explicit and ensure that this file is copied into the container.

## Compose sample application
### PHP application with Apache2

Project structure:
```
.
├── docker-compose.yaml
├── app
    ├── Dockerfile
    └── index.php

```

[_docker-compose.yaml_](docker-compose.yaml)
```
services:
  web:
    build: app
    ports: 
      - '80:80'
    volumes:
      - ./app:/var/www/html/
```

## Deploy with docker-compose

```
$ docker-compose up -d
Creating network "php-docker_web" with the default driver
Building web
Step 1/6 : FROM php:7.2-apache
...
...
Creating php-docker_web_1 ... done

```

## Expected result

Listing containers must show one container running and the port mapping as below:
```
$ docker ps
CONTAINER ID        IMAGE                        COMMAND                  CREATED             STATUS              PORTS                  NAMES
2bc8271fee81        php-docker_web               "docker-php-entrypoi…"   About a minute ago  Up About a minute   0.0.0.0:80->80/tc    php-docker_web_1
```

After the application starts, navigate to `http://localhost:80` in your web browser or run:
```
$ curl localhost:80
Hello World!
```

Stop and remove the containers
```
$ docker-compose down
```

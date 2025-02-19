https://github.com/mitshel/sopds.git


# Introduction

Dockerfile to build a Simple OPDS server docker image. This fork is intended to be used with k8s.

http://www.sopds.ru

# Installation

Pull the latest version of the image from the docker.

```
docker pull a1gal/docker-sopds
```

Alternately you can build the image yourself.

```
docker build -t a1gal/docker-sopds https://github.com/a-gal/docker-sopds.git
```

# Quick Start

Run the image. You have to store postgresql database on external storage.

```
docker run --name sopds -d \
   --volume /path/to/library:/library:ro \
   --env 'DB_USER=sopds' \
   --env 'DB_NAME=sopds' \
   --env 'DB_PASS=sopds' \
   --env 'DB_HOST=""' \
   --env 'DB_PORT=""' \
   --publish 8081:8001 \
   a1gal/docker-sopds
```

This will start the sopds server and you should now be able to browse the content on port 8081.


# Create superuser

By default the superuser will be created with predefined name "admin" and password "admin". But you can manage it via appropriate environmental variables:
```bash
docker run --name sopds -d \
   --volume /path/to/library:/library:ro \
   --env 'DB_USER=sopds' \
   --env 'DB_NAME=sopds' \
   --env 'DB_PASS=sopds' \
   --env 'DB_HOST=""' \
   --env 'DB_PORT=""' \
   --env 'SOPDS_SU_NAME="your_name_for_superuser"' \
   --env 'SOPDS_SU_EMAIL='"your_mail_for_superuser@your_domain"' \
   --env 'SOPDS_SU_PASS="your_password_for_superuser"' \
   --publish 8081:8001 \
   a1gal/docker-sopds
```

# Scan library

```bash
docker exec -ti sopds bash
python3 manage.py sopds_util setconf SOPDS_SCAN_START_DIRECTLY True
```

# Autostart of the SOPDS Telegram-bot

By default the Telegram-bot isn't enabled. But you can configure it to be started with container start at any time. 
```bash
docker run --name sopds -d \
   --volume /path/to/library:/library:ro \
   --env 'DB_USER=sopds' \
   --env 'DB_NAME=sopds' \
   --env 'DB_PASS=sopds' \
   --env 'DB_HOST=""' \
   --env 'DB_PORT=""' \
   --env 'SOPDS_TMBOT_ENABLE="True"' \
   --publish 8081:8001 \
   a1gal/docker-sopds
```
Please don't forget to configure the bot itself via interface of SOPDS.

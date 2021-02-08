# Deprecated

This repository is archived and deprecated in favor of the new version in taigaio/taiga-docker

Please go to https://resources.taiga.io/30min-setup/ for instructions on how to setup with docker and go to https://taigaio.github.io/taiga-doc/dist/#upgrading-guide for instructions on upgrading to Taiga6.

# The fork

This is a fork from [devinsolutions/taiga](https://github.com/devinsolutions/docker-taiga) image,
which now is being officially maintained by Taigaio.

# Taiga Docker Image

This is based on the official Python image (Alpine variant) and combines
taiga-back and taiga-front components into a single container, which uses
uWSGI to serve them both.

## Deployment

Consult [Taiga: Setup production environment](https://taigaio.github.io/taiga-doc/dist/setup-production.html) to learn about external dependencies and basic configuration options. A very basic deployment example can be found in `docker-compose.yml` and an advanced one in `docker-compose.advanced.yml`.

### Configuring taiga-back

taiga-back can be configured using `/etc/opt/taiga-back/settings.py`. See `root/etc/opt/taiga-back/settings.py` in this repository for the default configuration and information about all the settings.

### Configuring taiga-front

taiga-front can be configured using `/etc/opt/taiga-front/conf.json`. See [conf.example.json](https://github.com/taigaio/taiga-front/blob/stable/conf/conf.example.json) for the default configuration.

### Configuring uWSGI

uWSGI can be configured using `/usr/local/etc/uwsgi/uwsgi.ini` and/or using [environmental variables](https://uwsgi-docs.readthedocs.io/en/latest/Configuration.html#environment-variables).

This file provides only the basic configuration, since settings defined in it cannot be overridden using environmental variables. Also, using environmental variables is the easiest way to extend the default configuration without the need duplicate contents on the configuration file.

See `Dockerfile` to learn about the variables exported by default and their significance.

#### Graceful shutdown

With the default configuration, uWSGI is shutdown forcefully on `SIGTERM` and gracefully on `SIGHUP`.

### Persistence

taiga-back persists data such as attachments in `/srv/taiga-back/media`.  **This directory is not a volume by default!**

The database is persisted in a named volume, managed directly by docker.

### Populate the database with initial data

You can populate the database by using `populate-db` command. Because this command will overwrite existing data, it is not run by default.
This command creates an initial user `admin` with password `123123` that you should change inmediately.
Once you've have populated the initial data, disable this line in the docker-compose.yml (`- populate-db`).

## Stability

Breaking changes may occur between different image tags, so make sure to review the changes before upgrading. Images tagged with respective Taiga version are guaranteed to be stable.

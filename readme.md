# Web and Database Docker setup

## Setup

Initializing the docker setup. Requires `docker` and `docker-compose`,
I will assume they are already installed

```bash
# create the env file
cp .env.example .env.local
# fill in the missing values

docker-compose up

sudo mkdir ./pgadmin-data/sessions/
sudo mkdir ./pgadmin-data/storage/
sudo mkdir ./pgadmin-data/azurecredentialcache/
sudo chmod a+rwx ./db-data/ ./pgadmin-data/ ./pgadmin-data/*
```

## Connecting in PGAdmin

First we need to get the local IP of the container (needs `jq` installed)

```bash
docker-compose up

docker network inspect <network-name> |
jq -r ".[0].Containers | to_entries[] | select(.value.Name == \"db\") | .value.IPv4Address"
```

This prints out an IP address. You can safely ignore the part after the `/`.
In PGAdmin, click `add new server` then give it a name.
Afterwards, click the connection tab.
There you paste the IP address into the `host name/address` box.
You can then save it.

## seeding the database

```bash
# download the zip file from the brightspace

# I will assume it is in ~/Downloads/imdb_full_wdt.zip
unzip ~/Downloads/imdb_full_wdt.zip -d .

# you need to install psql, example in arch:
sudo pacman -S postgresql-libs

# this command will seed the db, be prepared for it taking a while
psql -h localhost -p 5432 -U admin --dbname "imdb" -f imdb_full_wdt.sql
```

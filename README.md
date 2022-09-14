# Project Live Helper Chat

## Requirements
- git 2.2 or higher
- docker 20 or higher
- docker-compose 1.20 or higher
- Configure your public and private keys with gitlab to always use SSH with GIT
- Adrenaline and good passion for the code

## Instructions

This is dockerized version of Live Helper Chat. It includes these images

* `web` - nginx service
* `php` - php-fpm service
* `cobrowse` - co browsing running NodeJS service
* `php-cronjob` - cron jobs running service
* `php-resque` - php-resque worker running service
* `nodejshelper` - NodeJS Helper NodeJS running service
* `redis` - Redis service
* `db` - Database service

## Docker instructions

* Checkout the repository
* Run `git clone git@github.com:marcotorres/livechat.git docker-standalone`
* Run `cd docker-standalone`
* Copy `.env.default` to `.env`
* Edit `.env` file and change `LHC_SECRET_HASH` to any random string
* Database default settings if you don't change those in `.env` file.
  * Host - `db` 
  * Database name - `lhc_database`
  * Database username - `lhc_usr`
  * Database password - `lhc_password`
* For standard version without NodeJS plugin run
     * Run `install.sh` this will check out Live Helper Chat and required extensions
     * Run `docker-compose -f docker-compose-standard.yml pull && docker-compose -f docker-compose-standard.yml up`
* For version with NodeJS plugin
     * Run `install-nodejs.sh` this will check out Live Helper Chat and required extensions
     * Run `docker-compose -f docker-compose-nodejs.yml pull && docker-compose -f docker-compose-nodejs.yml up`

* Navigate to localhost:8081 and follow install instructions.

At first install steps you might need to run these commands to change folders permissions.

```shell
docker exec -it dockerstandalone_web_1 chown -R www-data:www-data /code/cache
docker exec -it dockerstandalone_web_1 chown -R www-data:www-data /code/settings
docker exec -it dockerstandalone_web_1 chown -R www-data:www-data /code/var
```

or change permission of these folders

```
livehelperchat/lhc_web/cache
livehelperchat/lhc_web/settings
livehelperchat/lhc_web/var
```
## For Traefik 

Create network

```shell
docker network create --gateway 192.168.90.1 --subnet 192.168.90.0/24 traefik
```

RUN
```shell
cp traefik/acme.json.example traefik/acme.json && \
cp traefik/logs/traefik.log.example traefik/logs/traefik.log && \
cp traefik/shared/.htpasswd.example traefik/shared/.htpasswd && \
chmod 0600 traefik/acme.json
```

## How to listen on standard 80 port?

Edit `.env` file and set `LHC_PUBLIC_PORT` and `LHC_NODE_JS_PORT` port to `80`

## How to set up HTTPS?

That's up to you. You can have in host machine running nginx and just proxy request or tweak images/docker files I provided. You should play around with `web` service.

## My mails are not sending?

You have to edit back office mail settings and use SMTP.

## After install todo

* Go to `Settings -> Live help confgiuration -> Chat configuration -> (Screen sharing)` and
    * Check `NodeJs support enabled`
    * In `socket.io path, optional` enter `/wsnodejs/socket.io`

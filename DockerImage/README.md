# Runalyze-Docker

Docker samples/templates for self-hosted RUNALYZE.

On the [RUNALYZE docs](https://github.com/Runalyze/docs) and [RUNALYZE admin-docs](https://github.com/Runalyze/admin-docs) you can find documents for installing RUNALYZE 4.

This Fork is provided AS-IS.  The upstream repo provided help, but left a lot of things out needed to get working.  This is working in my environment, but I am not a Docker/devops guru so chatGPT helped write a lot of  the things missing in the image. I'm sure some of it could be done way better :)



## Build


```
mkdir runalyze-app
cd runalyze-app
git clone -b master https://github.com/Gloopzlemeep/Runalyze-Tools.git
cp Runalyze-Tools/DockerImage/docker-compose.yml docker-compose.yml
cp Runalyze-Tools/DockerImage/.env.example .env
docker build Runalyze-Tools/DockerImage/. -t myrunalyze:latest
 ```


## Install


Make edits to docker-compose.yml where required. 

Most of the configuration should be done through environment variables in .env

```
docker-compose up -d
``` 

After you succesfully bringing the stack up, navigate to http://<host>/install to complete the app installation


## Update

Be carful! Upgrading to a new version may require data migration. See runalyze repo for details

```
docker-compose pull && docker-compose up -d
``` 

To clean up unused images

```
docker image prune
``` 

## Environment Variables

The following ENV variables are avaialbe

| Environment Variable                      | Default                           |
|----------------------------------------- | ----------------------------------|
| LOCALE                                    | en                                |
| DATABASE_HOST                             | 127.0.0.1                         |
| DATABASE_PREFIX                           | runalyze_                         |
| DATABASE_PORT                             | 3306                              |
| DATABASE_NAME                             | runalyze                          |
| DATABASE_PASSWORD                         | test                              |
| DATABASE_USER                             | runalyze_test                     |
| SECRET                                   | please_change_this_secret          |
| UPDATE_DISABLED                           | no                               |
| USER_CAN_REGISTER                         | true                             |
| USER_DISABLE_ACCOUNT_ACTIVATION           | false                            |
| MAINTENANCE                               | false                            |
| GARMIN_API_KEY                            |                                  |
| WEATHER_PROXY                             |                                  |
| OPENWEATHERMAP_API_KEY                    |                                  |
| METEOSTATNET_API_KEY                      |                                  |
| DARKSKY_API_KEY                           |                                  |
| NOKIA_HERE_APPID                          |                                  |
| NOKIA_HERE_TOKEN                          |                                  |
| THUNDERFOREST_API_KEY                     |                                  |
| MAPBOX_API_KEY                            |                                  |
| GEONAMES_USERNAME                         |                                  |
| PERL_PATH                                | /usr/bin/perl                     |
| PYTHON3_PATH                              | /usr/bin/python3                   |
| RSVG_PATH                                | /usr/bin/rsvg-convert              |
| INKSCAPE_PATH                             | /usr/bin/inkscape                  |
| TTBIN_PATH                               | ../call/perl/ttbincnv             |
| SQLITE_MOD_SPATIALITE                    | libspatialite.so.5                |
| MAIL_SENDER                              |                                  |
| MAIL_NAME                                |                                  |
| SMTP_HOST                                | localhost                         |
| SMTP_PORT                                | 25                               |
| SMTP_SECURITY                            |                                  |
| SMTP_USERNAME                            |                                  |
| SMTP_PASSWORD                            |                                  |
| MAIL_LOCALDOMAIN                         |                                  |
| BACKUP_STORAGE_PERIOD                     | 5                                |
| POSTER_STORAGE_PERIOD                     | 5                                |
| ROUTER_REQUEST_CONTEXT_HOST               | localhost                         |
| ROUTER_REQUEST_CONTEXT_SCHEME             | http                             |
| ROUTER_REQUEST_CONTEXT_BASE_URL           |                                  |
| OSM_OVERPASS_URL                         |                                  |
| OSM_OVERPASS_PROXY                       |                                  |


## Issues

* Reverse Proxy
For some reason I could not get this to cooperate when running behind a reverse proxy that was terminating TLS. So the container listens on both 80/443 and has a self signed cert for 443.  When doing this, I was succesfully able to enable TLS.

* Email 
I wasn't able to get email working but also don't plan on using it. I'm not sure if it was config issues on my end, or if the container is missing some dependencies. To get around this I would recommend having USER_DISABLE_ACCOUNT_ACTIVATION set to false, as you cannot login without activating your account. 

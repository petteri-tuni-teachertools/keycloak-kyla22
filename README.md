# Introduction

This project is based on this demo project for **KeyCloak**: 
https://github.com/stianst/keycloak-containers-demo that consists of


* Keycloak authentication server - somewhat old (9.x) version
* LDAP server
* JS console for demonstrating **OpenID Connect** authentication 

Demo **Mailhog** mail server can be used for sending and receiving mail.

# Access / URLs

* Keycloak auth server Web: https://iam22.ilab.fi:8443 (admin / admin)
* JS Console for login demo: http://iam22.ilab.fi:8000/
* LDAP-server: ldap://iam22.ilab.fi:389
* Mailhog (optional): 
  * Web: http://iam22.ilab.fi:8025
  * SMTP: port 1025 (demo-mail docker hostname)

No test users by default. Add from Keycloak web or by self registration.

# Development history 

The basic project has then been forked and modified. The aims are:

1) Support operation as a server (original runs onlyl in localhost)
2) Use **docker compose** for easier deployment

More aims:

3) Configure real mail sending - rather than local Mailhog demo server 
4) Real certificate
5) "Fatter" original configuration - alleviate initial manual configuration
6) Backup restore

# Status

At the moment, steps 1) and 2) have been completed. Others still under construction.

# Getting started

1) Clone the repo
2) Change to repo root directory
2) Up with docker compose
3) Start mailhog (if needed)

Clone
````
git clone git@github.com:petteri-tuni-teachertools/keycloak-kyla22.git
cd keycloak-kyla22
````
Docker compose up:
````
docker compose up -d
````

Mailhog docker up (optional)
````
docker run -d -p 8025:8025 -p 1025:1025 --name demo-mail --net keycloak-kyla22_default mailhog/mailhog
````
This format exposes both HTTP and SMTP ports outside (not necessary to expose 1025). Network name comes from starting the main application - use the name of that + _default

# Manual configuration

See the **README-ORIG.md** for how to configure the Demo Realm etc. manually from Keycloak Web Interface.

# Technical details

## Typical test users:

User / Password / email

* u / p / u@localhost
* u1 / p / u1@localhost

## Docker commands (modifications):

### Docker cli console:
``` 
docker exec -it e7c55199be2c /bin/bash
``` 
### LDAP

To expose LDAP outside of container:

``` 
docker build -t demo-ldap ldap
docker run --name demo-ldap -p 389:389 --net demo-network demo-ldap
``` 

## Docker compose instructions

### Network

By default the name of the network is the name of the application (parent directory) + _default. For this project it is **keycloak-kyla_default**.

Project name can be modified with with either the --project-name flag or the COMPOSE_PROJECT_NAME environment variable.

See here: https://docs.docker.com/compose/networking/ 

### Commands

To backgroupnd and force recreate:
``` 
docker compose up -d --force-recreate
``` 
Status
``` 
docker compose ps
``` 
Start/Stop/Up/Down
``` 
docker compose up/down/start/stop/restart
``` 
The **up** creates and **down** destroys. Use **start** and **stop** to keep the container.


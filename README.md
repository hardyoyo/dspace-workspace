# DSpace Workspace

An example [Lando](https://lando.dev/download/) dev environment for DSpace 7.

## Description

This is a rough, but ultimately _working_ example of a Lando dev environment for DSpace 7.
It's a good starting point, but might need further tweaking to get just right.

## How to use:

You'll need to clone the [DSpace 7 API project](https://github.com/DSpace/dspace/),
as well as the [DSpace-Angular front-end](https://github.com/DSpace/dspace-angular/), 
into this folder. Ideally, you want to make your own fork of both of these 
projects before you clone them. That's up to you. The examples below will use the 
parent GitHub repositories for both of these projects. I don't recommend actually 
doing this, but it's good enough for this README.

```
git clone https://github.com/DSpace/DSpace.git
git clone https://github.com/DSpace/dspace-angular.git
```
## Tooling

DSpace-specific tooling for this project includes:

```
  lando ant               Runs Ant commands on the DSpace apiserver
  lando catalina.sh       Runs catalina.sh commands
  lando copy-solr-cores   Copies DSpace Solr cores to the Solr data path
  lando dspace            Runs dspace commands
  lando mvn               Runs Maven commands on the DSpace apiserver
  lando psql              Drops into a psql shell on the database service
  lando restart-tomcat    Restarts Tomcat (faster than lando restart)
  lando yarn              Invokes yarn on the DSpace-Angular frontend servicee
```

## Initial setup steps
1. ensure you have cloned both dspace and dspace-angular to this project folder
2. `lando rebuild -y`
3. `lando mvn clean package`
4. `lando ant fresh_install`
5. `lando copy-solr-cores`
6. `lando restart`
7. `lando dspace database migrate` (shouldn't be necessary, but is)
8. `lando dspace create-administrator`
9. pause to admire your dev APIserver running at: (`lando info` will tell you)
10. `lando yarn install`
11. `lando yarn start`
12. play with your new DSpace-Angular frontend, pointed at your dev APIserver

## Tomcat Configuration
All the tomcat configs are in the `tomcat_config` folder. Edits to them 
require a `lando restart-tomcat` or a `lando restart` for Tomcat to pick 
up the changes. If you change the Tomcat version in the landofile, you'll 
need to copy over the default configs to this folder. You should be able 
to figure that out on your own, if you're bothering to change the Tomcat 
version.
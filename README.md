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
2. **configure your local.cfg file** (see below)
3. `lando rebuild -y`
4. `lando mvn clean package`
5. `lando ant fresh_install`
6. `lando copy-solr-cores`
7. `lando restart`
8. `lando dspace database migrate` (shouldn't be necessary, but is)
9. `lando dspace create-administrator`
10. pause to admire your dev APIserver running at: (`lando info` will tell you)
11. `lando yarn install`
12. `lando yarn start`
13. play with your new DSpace-Angular frontend, pointed at your dev APIserver

NOTE: As you work with this dev environment, you may need to rebuild it. The contents of the `dist` folder
will remain as-is, and may contain incorrect configuration files. The best way to ensure this doesn't happen
is, immediately after you run `lando yarn install` run `lando yarn run clean:dist`. This will clean up the
dist folder for you. You can also run `lando yarn run clean` if you wish to really clean everything out, just
know the full clean process also deletes everything that `lando yarn install` installed, so you'll need to
run `lando yarn install` again after running `lando yarn run clean`. ALSO, you won't be able to run any of
the yarn scripts if you haven't run `lando yarn install` ... so don't try to clean *before* you install,
because that won't work at all.

## local.cfg file
You should start with the example local.cfg.EXMAPLE file that comes with DSpace, and then ensure the following values are set correctly:
```
dspace.dir=/app/dspace_home
dspace.server.url = http://dspace-api.local.sandbox/server # requires Lando proxy
dspace.ui.url = http://dspace.local.sandbox # requires Lando proxy
dspace.name = LANDO DSpace
db.url = jdbc:postgresql://database:5432/dspace
db.username = postgres
db.password = 
```

This file is super important to keeping this whole dev environment working. If 
your dev environment suddenly stops working correctly, first check to be sure 
this file is present. It's in the `.gitignore` file for dspace, so... git won't
keep track of it for you. So... from time to time it might go missing, depending
on whatever else you might be working on. 
## Tomcat Configuration
All the tomcat configs are in the `tomcat_config` folder. Edits to them 
require a `lando restart-tomcat` or a `lando restart` for Tomcat to pick 
up the changes. If you change the Tomcat version in the landofile, you'll 
need to copy over the default configs to this folder. You should be able 
to figure that out on your own, if you're bothering to change the Tomcat 
version.

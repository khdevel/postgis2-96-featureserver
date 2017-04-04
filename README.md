# postgis2-96_featureserver
A Dockerfile with setup of postgis2 and featureserver

## Base Docker Image

* [centos:latest](https://hub.docker.com/_/centos/)

## Image Contents

* [postgresql96](https://yum.postgresql.org/repopackages.php#pg96) - database
* [postgis2-96](http://postgis.net) - pgsql extension to support for geographic objects
* [featureserver](http://featureserver.org/index.html) -  RESTful Geographic Feature Service
* [virtualenv](https://virtualenv.pypa.io/en/stable/) - for featureserver's python modules purpose

## The Idea

The idea was to setup a Docker image with installed above components on [amazonlinux](https://hub.docker.com/_/amazonlinux/). Unfortunately there were lot of problems during *postgis2_96* installation, mainly with dependencies.
To prevent time wasting I just switched to ordinary *Centos7* where I did not notice problems like on *amazonlinux*. Of course there were others but more "user friendly" :)

## Knowledge

Below topics were very helpful!

* [Installing pyspatialite](http://blog.oddbit.com/2015/11/17/installing-pyspatialite-on-fedora/)
* [virtualenv - User Guide](https://virtualenv.pypa.io/en/stable/userguide/)

## Work Done

1. Docker image with the latest *Centos7*
2. Installed and configured *postgresql96*
3. Installed *postgis2-96*
4. Installed *featureserver*

## Run with docker-compose

Just run like that:

`docker-compose up`

To rebuild it:

`docker-compose up --build`

## Check if featureserver works

Featureserver is running in the *standalone* mode. Open the web browser and type:

`http://localhost:8080`

You should see in the docker-compose logs:

```
postgis2         | pg_ctl: another server might be running; trying to start server anyway
postgis2         | waiting for server to start....< 2017-04-04 15:12:17.672 UTC >LOG:  redirecting log output to logging collector process
postgis2         | < 2017-04-04 15:12:17.672 UTC >HINT:  Future log output will appear in directory "pg_log".
postgis2         |  done
postgis2         | server started
postgis2         | Creating user "bar"...
postgis2         | CREATE ROLE
postgis2         | Creating database "foo"...
postgis2         | CREATE DATABASE foo;
postgis2         | CREATE DATABASE
postgis2         | Granting access to database "foo" for user "bar"...
postgis2         | GRANT
postgis2         | waiting for server to shut down.... done
postgis2         | server stopped
postgis2         | Starting PostgreSQL 9.6 server...
postgis2         | < 2017-04-04 15:12:20.517 UTC >LOG:  redirecting log output to logging collector process
postgis2         | < 2017-04-04 15:12:20.517 UTC >HINT:  Future log output will appear in directory "pg_log".
featureserver    | 192.168.99.1 - - [04/Apr/2017 15:12:24] "GET / HTTP/1.1" 200 35
```

## TODO

1. ~~Create a *docker-compose* file~~
2. Test of *featureserver* features
  * ~~Start the featuresrever~~
  * ~~Connect the featureserver to postgis2~~
3. Find a way to run it on *amazonlinux*
4. Make the image smaller
5. Refactor scripts in the data/ directory

## Acknowledgement

Most of this code was refactored from great [zokeber/docker-postgresql](https://github.com/zokeber/docker-postgresql) repository. Please go give him stars and likes -- he deserves them!

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

## Run

### Setup the postgresql

Below description covers the task related to the pgsql setup.

**Build the image**

Create the image *khdevel/featureserver* by typing below command:

`docker build -t khdevel/featureserver:0.1 .`

**Run the postgresql server**

To run the pgsql as a daemon type below command:

`docker run -d -p 5432:5432 --name featureserver96 khdevel/featureserver:0.1`

**Connect to the pgsql**

`docker exec -it featureserver96 psql`

### Creating a database and username

To create a postgresql database and a user just use `DB_NAME`, `DB_USER` and `DB_PASS` variables.

**Creation**

Type below to create a DB_USER=gooduser, DB_PASS=passwd and DB_NAME=foo:

```
docker create -it -p 5432:5432 --name featureserver96 --env 'DB_USER=gooduser' --env 'DB_PASS=passwd' --env 'DB_NAME=foo' khdevel/featureserver:0.1
```

**Connection**

To connect to created database just type:

`docker start featureserver96`

and then...

`docker exec -it featureserver96 psql foo -U gooduser`

## TODO

1. Create a *docker-compose* file
2. Test of *featureserver* features
3. Find a way to run it on *amazonlinux*
4. Make the image smaller
5. Refactor scripts in the data/ directory

## Acknowledgement

Most of this code was refactored from great [zokeber/docker-postgresql](https://github.com/zokeber/docker-postgresql) repository. Please go give him stars and likes -- he deserves them!

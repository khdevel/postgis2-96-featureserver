#
# postgresql + postgis2 on CentOS 7
#
# Pull base image
FROM centos:latest

# Postgresql version
ENV PG_VERSION 9.6
ENV PGVERSION 96

# Set the environment variables
ENV HOME /var/lib/pgsql
ENV PGDATA /var/lib/pgsql/$PG_VERSION/data

# Install postgresql and the latest EPEL repository
RUN rpm -vih https://download.postgresql.org/pub/repos/yum/$PG_VERSION/redhat/rhel-7-x86_64/pgdg-centos$PGVERSION-$PG_VERSION-3.noarch.rpm && \
    yum install -y epel-release && \
    yum update -y

# Install core packages
RUN yum install -y \
    sudo \
    pwgen \
    gcc \
    python-devel \
    python2-pip \
    sqlite-devel \
    geos-devel \
    proj-devel \
    redhat-rpm-config \
    postgis2_$PGVERSION \
    postgis2_$PGVERSION-client \
    postgis2_$PGVERSION-devel \
    postgis2_$PGVERSION-utils \
    postgresql$PGVERSION-server \
    postgresql$PGVERSION-contrib && \
    yum clean all

# Copy the postgresql-setup
COPY data/postgresql-setup /usr/pgsql-$PG_VERSION/bin/postgresql$PGVERSION-setup

# Change the WORKDIR
WORKDIR /var/lib/pgsql

# Run initdb
RUN /usr/pgsql-$PG_VERSION/bin/postgresql$PGVERSION-setup initdb

# Copy config file
COPY data/postgresql.conf /var/lib/pgsql/$PG_VERSION/data/postgresql.conf
COPY data/pg_hba.conf /var/lib/pgsql/$PG_VERSION/data/pg_hba.conf
COPY data/postgresql.sh /usr/local/bin/postgresql.sh

# Change own user
RUN chown -R postgres:postgres /var/lib/pgsql/$PG_VERSION/data/* && \
    usermod -G wheel postgres && \
    sed -i 's/.*requiretty$/#Defaults requiretty/' /etc/sudoers && \
    chmod +x /usr/local/bin/postgresql.sh

# Set volume
VOLUME ["/var/lib/pgsql"]

# Set username
USER postgres

# Run PostgreSQL Server
CMD ["/bin/bash", "/usr/local/bin/postgresql.sh"]

# Expose port
EXPOSE 5432

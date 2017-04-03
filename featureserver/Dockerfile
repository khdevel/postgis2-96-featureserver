#
# featureserver Dockerfile on CentOS 7
#
# Pull base image
FROM centos:latest

# Install postgresql and the latest EPEL repository
RUN yum install -y epel-release && \
    yum update -y

# Install core packages
RUN yum install -y \
    sudo \
    pwgen \
    gcc \
    git \
    python-devel \
    python2-pip \
    sqlite-devel \
    geos-devel \
    proj-devel \
    redhat-rpm-config && \
    yum clean all

# Install the virtualenv
RUN sudo -H pip install virtualenv

# Change the WORKDIR
WORKDIR /opt

# Install featureserver from virtualenv
RUN virtualenv featureserver
WORKDIR /opt/featureserver
RUN sudo -H ./bin/pip install \
    dxfwrite \
    lxml \
    Cheetah \
    simplejson \
    shortuuid \
    psycopg2
RUN CFLAGS=-I/usr/include ./bin/pip install pyspatialite

# Clone featureserver
RUN git clone https://github.com/iocast/featureserver.git

# Run featureserver Server
CMD ["/opt/featureserver/bin/python", "featureserver/featureserver_http_server.py"]

# Expose port
EXPOSE 8080

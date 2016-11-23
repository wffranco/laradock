FROM ubuntu:14.04

MAINTAINER Latchezar Hristov

# ADD setup_5.x /tmp/setup_5.x
# RUN bash /tmp/setup_5.x

RUN apt-get update
RUN apt-get install -y npm git \
    && ln -s /usr/bin/nodejs /usr/bin/node
RUN /usr/bin/npm install -g gulp
RUN /usr/bin/npm install -g bower

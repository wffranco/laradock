FROM debian:stable

MAINTAINER Latchezar Hristov

WORKDIR /tmp/node-build

# ADD setup_5.x /tmp/setup_5.x
# RUN bash /tmp/setup_5.x

RUN apt-get update
RUN apt-get install -y npm \
    && ln -s /usr/bin/nodejs /usr/bin/node
RUN /usr/bin/npm install -g gulp
RUN /usr/bin/npm install -g bower

VOLUME ["/tmp/node-build"]
RUN /usr/bin/npm install gulp
# CMD ["gulp", "client"]
CMD ["/bin/bash"]

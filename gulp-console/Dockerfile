FROM node:0.12.10

MAINTAINER Latchezar Hristov

ENV HOME=/home/tms

RUN useradd --user-group --create-home --shell /bin/false tms &&\
  npm install --global npm@2.14.9 &&\
  npm install --global bower &&\
  npm install --global gulp

WORKDIR $HOME

COPY ./client $HOME/client
COPY ./super $HOME/super
COPY ./school $HOME/school
COPY ./config $HOME/config
COPY ./gulpfile.js ./package.json $HOME/
COPY ./laradock/gulp-console/docker.json $HOME/config/docker.json

RUN chown -R tms:tms $HOME/*

RUN chown -R tms:tms $HOME/.npm

USER tms
RUN npm install

RUN cd $HOME/client && bower install && bower update

RUN npm install gulp

USER root
RUN chown -R tms:tms $HOME/*

# RUN gulp client
# RUN cd /tmp/node-build && gulp client --env docker
# CMD ["/bin/bash"]

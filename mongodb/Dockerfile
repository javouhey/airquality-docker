# Dockerfile for mongodb
# Used for https://github.com/javouhey/airquality

FROM ubuntu:latest
MAINTAINER Gavin Bong <rubycoder@gmail.com>

RUN locale-gen --no-purge en_US.UTF-8
ENV LC_ALL en_US.UTF-8

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
RUN echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | tee /etc/apt/sources.list.d/10gen.list

RUN apt-get update
RUN apt-get install -y mongodb-10gen

USER mongodb

# mount point for logs & data directory
VOLUME /mongowork

ADD ./mongodb.conf /etc/mongodb.conf

# mongodb process custom port
EXPOSE 44444

CMD ["--config", "/etc/mongodb.conf"]
ENTRYPOINT ["usr/bin/mongod"]

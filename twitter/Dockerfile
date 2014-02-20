# Dockerfile for reader of twitter feeds
# Used for https://github.com/javouhey/airquality

FROM ubuntu:precise
MAINTAINER Gavin Bong <rubycoder@gmail.com>

RUN locale-gen --no-purge en_US.UTF-8
ENV LC_ALL en_US.UTF-8

RUN mkdir -p /var/log/supervisord
RUN mkdir -p /twitter

RUN apt-get update
RUN apt-get install -y supervisor
RUN apt-get install -y unzip
RUN apt-get install -y wget
RUN apt-get install -y python-pip

VOLUME /mongowork

ADD ./supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# install twitter code
RUN wget --no-check-certificate https://github.com/javouhey/airquality/archive/master.zip
RUN unzip -d /twitter master.zip
RUN pip install -r /twitter/airquality-master/requirements.txt
RUN cd /twitter/airquality-master && python setup.py install

RUN apt-get clean

CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

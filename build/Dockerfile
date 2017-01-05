FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive
ENV TERM xterm

RUN apt-get update

RUN apt-get -y install apt-utils wget

RUN apt-get -y install mysql-server mysql-client

# apache
RUN apt-get -y install apache2 apache2-utils
ADD apache2.conf /etc/apache2/sites-available/000-default.conf

# php
ADD ondrejphp.list /etc/apt/sources.list.d/ondrejphp.list
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E5267A6C
RUN apt-get update
RUN apt-get -y install php5.6-cgi php5.6-cli php5.6 php5.6-xcache # php-pear
RUN apt-get -y install curl libcurl3 libcurl3-dev php5.6-curl
RUN apt-get -y install libapache2-mod-php5.6
ADD php.ini /etc/php/5.6/apache2/conf.d/10-majordomo.ini
ADD php.ini /etc/php/5.6/cli/conf.d/10-majordomo.ini

RUN a2enmod rewrite

# install majordomo
RUN echo "as-a"
RUN wget http://majordomo.smartliving.ru/download/_majordomo_linux_100b.tar.gz -O /tmp/majordomo.tar.gz
RUN mkdir /tmp/majordomo
RUN tar xvfz /tmp/majordomo.tar.gz -C /tmp/majordomo
RUN rm -rf /var/www
RUN mv /tmp/majordomo/html /var/www
#RUN rm -rf /tmp/majordomo*

RUN chown -R www-data:www-data /var/www
RUN usermod -a -G audio www-data

# configure database
RUN echo "mysql ----  "
RUN service mysql start && mysql -e "CREATE DATABASE db_terminal /*\!40100 DEFAULT CHARACTER SET utf8 */;"

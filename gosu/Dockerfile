FROM ubuntu:16.04
MAINTAINER WON


## Setup Environment
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -y

## Ensure UTF-8
RUN apt-get install -y locales
RUN locale-gen  en_US.UTF-8
ENV LANG        en_US.UTF-8
ENV LC_ALL      en_US.UTF-8


## UTC TIME -> KST
RUN ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime

## Prepare apt-repository
RUN apt-get install -y software-properties-common
RUN add-apt-repository -y ppa:ondrej/php
RUN add-apt-repository -y ppa:nginx/stable

## Update Source
RUN apt-get update -y

## install packages
RUN apt-get install -y vim bash-completion unzip curl less sudo
RUN apt-get install -y git

## install other tools
RUN apt-get install net-tools

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash
RUN apt-get install -y nodejs

## install supervisord
RUN apt-get install -y supervisor
RUN mkdir -p /var/log/supervisor/conf.d/
ADD supervisord.conf /etc/supervisor/conf.d/


## install php
RUN apt-get install -y php7.1
RUN apt-get install -y php7.1-fpm
RUN apt-get install -y php7.1-cli php7.1-gd php7.1-curl php7.1-mbstring php7.1-xml php7.1-zip php7.1-mysql


# nginx
RUN apt-get install -y nginx
RUN rm /etc/nginx/sites-enabled/default

## php timezone setting
ADD php-fpm/php-ini-overrides.ini /etc/php/7.1/fpm/conf.d/99-overrides.ini
ADD php-fpm/php-ini-overrides.ini /etc/php/7.1/cli/conf.d/99-overrides.ini

## adduser
RUN adduser --disabled-password --gecos "" w
RUN usermod -a -G www-data w
RUN usermod -a -G sudo w
RUN echo w ALL=NOPASSWD: ALL >> /etc/sudoers


## nginx config
ADD nginx/web /etc/nginx/sites-available/
RUN ln -s /etc/nginx/sites-available/web /etc/nginx/sites-enabled/web

ADD nginx/nginx.conf /etc/nginx/conf.d

RUN service php7.1-fpm start

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

USER w
RUN mkdir -p /home/w/.ssh && chmod 700 /home/w/.ssh

WORKDIR /app

EXPOSE 80 443

VOLUME ["/app"]

CMD ["sudo", "supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]

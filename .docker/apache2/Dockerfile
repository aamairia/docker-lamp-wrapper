FROM debian:bookworm
MAINTAINER Aymen AMAIRIA <amairia.aymen@gmail.com>
USER root

ENV HTTPD_PREFIX /usr/local/apache2
ENV PATH $HTTPD_PREFIX/bin:$PATH
RUN mkdir -p "$HTTPD_PREFIX" \
    && chown www-data:www-data "$HTTPD_PREFIX"
WORKDIR $HTTPD_PREFIX

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    apache2 curl openssl nano tcpdump binutils \
    && rm -r /var/lib/apt/lists/*
RUN a2enmod proxy_fcgi ssl rewrite proxy proxy_balancer proxy_http proxy_ajp ssl
RUN sed -i '/Global configuration/a \
ServerName localhost \
' /etc/apache2/apache2.conf
RUN a2dissite 000-default.conf

RUN apt-get update && apt-get -y upgrade && apt-get install -y \
    wget \
    curl \
    nano \
    vim \
    zip \
    unzip \
    dos2unix \
    apt-transport-https \
    lsb-release \
    locales

WORKDIR /var/www/html/app/

EXPOSE 80 443
RUN rm -f /run/apache2/apache2.pid
CMD apachectl  -DFOREGROUND -e info
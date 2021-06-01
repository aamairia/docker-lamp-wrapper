FROM debian:stretch
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

##PHP 7.3

RUN wget  https://packages.sury.org/php/apt.gpg
RUN mv apt.gpg /etc/apt/trusted.gpg.d/php.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php7.3.list

RUN apt-get update && apt-get -y upgrade && apt-get install -y \
    libapache2-mod-php7.3 \
	php7.3 \
	php7.3-cgi \
	php7.3-cli \
	php7.3-common \
	php7.3-curl \
	php7.3-dev \
	php7.3-enchant \
	php7.3-fpm \
	php7.3-gd \
	php7.3-gmp \
	php7.3-imap \
	php7.3-interbase \
	php7.3-intl \
	php7.3-json \
	php7.3-ldap \
	php7.3-mysql \
	php7.3-odbc \
	php7.3-pgsql \
	php7.3-phpdbg \
	php7.3-pspell \
	php7.3-readline \
	php7.3-recode \
	php7.3-sybase \
	php7.3-tidy \
	php7.3-xmlrpc \
	php7.3-xsl \
	php7.3-xdebug \
	php7.3-soap

#Composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN chmod +x /usr/local/bin/composer

#PHPUnit
RUN wget https://phar.phpunit.de/phpunit-6.5.phar
RUN chmod +x phpunit-6.5.phar
RUN mv phpunit-6.5.phar /usr/local/bin/phpunit

WORKDIR /var/www/html/app/

EXPOSE 80 443
RUN rm -f /run/apache2/apache2.pid
CMD apachectl  -DFOREGROUND -e info
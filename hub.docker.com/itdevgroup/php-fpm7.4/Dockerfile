################################
#                              #
#   Ubuntu - PHP 7.4 CLI+FPM   #
#                              #
################################

FROM ubuntu:bionic

MAINTAINER IT DEV GROUP <info@it-devgroup.com>

LABEL Vendor="itdevgroup"
LABEL Description="PHP-FPM v7.4"
LABEL Version="1.0.0"

ENV ITDEVGROUP_TIME_ZONE Europe/Kiev
RUN ln -snf /usr/share/zoneinfo/$ITDEVGROUP_TIME_ZONE /etc/localtime && echo $ITDEVGROUP_TIME_ZONE > /etc/timezone

RUN apt-get update -yqq \
    && apt-get install -yqq \
	ca-certificates \
    git \
    gcc \
    make \
    wget \
    mc \
    curl \
    cron \
    zip \
    supervisor 

RUN DEBIAN_FRONTEND=noninteractive apt-get -y dist-upgrade
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install software-properties-common
RUN DEBIAN_FRONTEND=noninteractive LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php

## Install php7.4
RUN apt-get update -yqq && apt-get install -yqq php7.4

## Install php7.4 extension
RUN apt-get update -yqq \
    && apt-get install -yqq \
	php7.4-pgsql \
	php7.4-mysql \
	php7.4-sqlite \
	php7.4-sqlite3 \
	sqlite3 \
	libsqlite3-dev \
	php7.4-opcache \
	php7.4-common \
	php7.4-mbstring \
	php7.4-soap \
	php7.4-cli \
	php7.4-intl \
	php7.4-json \
	php7.4-xsl \
	php7.4-imap \
	php7.4-ldap \
	php7.4-curl \
	php7.4-gd  \
        php7.4-zip  \
	php7.4-dev \
        php7.4-fpm \
	php-igbinary \
	php7.4-redis \
	php-memcached \
	php-mongodb \
	php7.4-imagick \
	php7.4-bcmath \
        php-zmq \
        php7.4-apcu \
    && apt-get install pkg-config \
    && apt-get install -y -q --no-install-recommends \
       ssmtp

# Add default timezone
RUN echo "date.timezone=$ITDEVGROUP_TIME_ZONE" > /etc/php/7.4/cli/conf.d/timezone.ini

# Download browscap.ini
RUN mkdir /var/lib/browscap \
    && wget http://browscap.org/stream?q=BrowsCapINI -O /var/lib/browscap/browscap.ini

## Install composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer -d=/var/www

# Install prestissimo for parallel composer downloads
RUN mkdir -p /var/www/.composer && chown -R www-data /var/www/.composer
USER www-data
RUN composer global require hirak/prestissimo \
 && rm -rf /var/www/.composer/cache
USER root
RUN composer global require hirak/prestissimo \
 && rm -rf /root/.composer/cache

# Copy our config files for php7.4 fpm and php7.4 cli
COPY php-conf/php.ini /etc/php/7.4/cli/php.ini
COPY php-conf/php-fpm.ini /etc/php/7.4/fpm/php.ini
COPY php-conf/php-fpm.conf /etc/php/7.4/fpm/php-fpm.conf
COPY php-conf/www.conf /etc/php/7.4/fpm/pool.d/www.conf

# Download and install wkhtmltopdf
RUN apt-get install xvfb libfontconfig wkhtmltopdf -y

## Create xvfb wrapper for wkhtmltopdf and create special sh script
RUN touch /usr/local/bin/wkhtmltopdf \
    && chmod a+x /usr/local/bin/wkhtmltopdf \
    && echo 'xvfb-run -a -s "-screen 0 640x480x16" wkhtmltopdf "$@"' > /usr/local/bin/wkhtmltopdf \
    && chmod a+x /usr/local/bin/wkhtmltopdf


RUN usermod -u 1000 www-data
RUN usermod -aG staff,users,www-data www-data
# Reconfigure system time
RUN  dpkg-reconfigure -f noninteractive tzdata

# Clear all packages and temp files
RUN	apt-get clean -yqq \
	&& apt-get purge php7.4-dev -yqq \
	&& apt-get purge git -yqq \
	&& apt-get purge gcc -yqq \
	&& apt-get purge make -yqq \
	&& apt-get purge wget -yqq \
	&& apt-get purge curl -yqq

RUN rm -rf /var/lib/apt/lists/* \
	&& rm -rf /tmp/* \
	&& apt-get clean -yqq

COPY start.sh /start.sh
RUN chmod +x /start.sh

CMD ["/start.sh"]

WORKDIR /var/www/itdevgroup

EXPOSE 9000

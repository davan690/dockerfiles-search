FROM php:7-apache
MAINTAINER Kerry Knopp <krknopp@gmail.com>

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libcurl4-openssl-dev \
	libexif-dev \
        libssl-dev \
        libpng-dev \
        libpq-dev \
        libxml2-dev \
        zlib1g-dev \
        libc-client-dev \
        libkrb5-dev \
        libldap2-dev \
        cron \
        vim \
        supervisor \
        git \
	mysql-client \
	ssmtp \
        && apt-get clean && a2enmod rewrite

RUN docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
        curl \
	exif \
        mysqli \
        mbstring \
        zip \
        ftp \
        gd \
        fileinfo \
        soap \
        zip \
        imap

# Install Confd
ADD https://github.com/kelseyhightower/confd/releases/download/v0.16.0/confd-0.16.0-linux-amd64 /usr/local/bin/confd
RUN chmod +x /usr/local/bin/confd

# Add Custom PHP and Apache configs
COPY confd /etc/confd/
COPY config/php.ini /usr/local/etc/php/
COPY config/000-default.conf /etc/apache2/sites-enabled/000-default.conf
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY bash_aliases /root/.bashrc
COPY scripts /root/

RUN (crontab -l 2>/dev/null; echo "*    *    *    *    *     cd /var/www/html; php -f cron.php > /dev/null 2>&1 ") | crontab -
RUN chown www-data:www-data /var/www/html/ -R

# Create volumes. Will change when build process is in place
VOLUME /var/www/html/ /mnt/sites-files

WORKDIR /var/www/site
EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]

from php:5-fpm

ADD piwigo /var/www/html

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
	libexif-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install  mysqli \
    && docker-php-ext-install exif

RUN sed -i "s@;php_flag.display_errors. = off@php_flag[display_errors] = on@" /usr/local/etc/php-fpm.d/www.conf 
RUN sed -i "s@;php_admin_value.error_log. @php_admin_value[error_log] @" /usr/local/etc/php-fpm.d/www.conf 
RUN sed -i "s@;php_admin_flag.log_errors. @php_admin_flag[log_errors] @" /usr/local/etc/php-fpm.d/www.conf 
RUN echo 'php_admin_value[date.timezone] = Europe/Paris' >> /usr/local/etc/php-fpm.d/www.conf
RUN sed -i 's/;php_admin_value.memory_limit. = 32M/php_admin_value[memory_limit] = 256M/' /usr/local/etc/php-fpm.d/www.conf 


RUN \
    apt-get update && \
    apt-get install libldap2-dev -y && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap


RUN chown -R www-data:www-data /var/www/html

ADD t.php /
ADD fpm-entrypoint.sh /fpm-entrypoint.sh

CMD ["/fpm-entrypoint.sh"]

FROM php:7.2-fpm-stretch

# SETTING ENVIRONMENT
ENV ENVIRONMENT docker

RUN apt-get update

# INSTALL PHP EXTENSION DEPENDENCIES
# intl
RUN apt-get install -y libicu-dev
# gmp
RUN apt-get install -y libgmp-dev

# INSALL PHP EXTENSIONS
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install intl
RUN docker-php-ext-install gmp

# INSTALL XDEBUG
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

# INSTALL TIDEWAYS PHP PROFILER
RUN mkdir /tmp/tideways && cd /tmp/tideways
RUN curl -L https://api.github.com/repos/tideways/php-xhprof-extension/tarball | tar xz --strip=1
RUN phpize
RUN ./configure
RUN make
RUN make install
RUN echo extension=tideways_xhprof.so > /usr/local/etc/php/conf.d/docker-php-ext-tideways.ini

# INSTALL MONGODB EXTENSION (NEEDED FOR TIDEWAYS)
RUN pecl install mongodb \
&& docker-php-ext-enable mongodb

# CUSTOM php.ini
RUN mkdir /usr/local/etc/php-docker && touch /usr/local/etc/php-docker/creiwork.ini
RUN ln -s /usr/local/etc/php-docker/creiwork.ini  /usr/local/etc/php/conf.d/creiwork.ini

# SANITIZE FOLDER STRUCTURE
RUN rm -r /var/www/html
WORKDIR /var/www
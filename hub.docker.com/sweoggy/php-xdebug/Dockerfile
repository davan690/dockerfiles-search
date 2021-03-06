FROM php:7.2

RUN apt update && apt install gnupg -y

RUN curl -sL https://deb.nodesource.com/setup_7.x | bash -
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN curl -sS https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-6.x.list

RUN apt update && apt upgrade -y && mkdir -p /usr/share/man/man1 && apt install openjdk-8-jre -y
RUN apt install git zlibc zlib1g zlib1g-dev libicu-dev libpng-dev nodejs yarn libpcre3-dev optipng elasticsearch -y

RUN mkdir -p /usr/share/man/man1 \ 
    && apt install procps openjdk-8-jre-headless -yqq \
    && echo "discovery.type: single-node" >> /etc/elasticsearch/elasticsearch.yml \
    && update-rc.d elasticsearch defaults 95 10

RUN curl -O https://mozjpeg.codelove.de/bin/mozjpeg_3.1_amd64.deb \ 
    && dpkg --install mozjpeg_3.1_amd64.deb \
    && apt install -f

RUN git clone --recursive https://github.com/pornel/pngquant.git \
    && cd pngquant \
    && ./configure \
    && make \
    && make install

RUN pecl install apcu \
    && pecl install xdebug \
    && docker-php-ext-enable apcu xdebug

RUN docker-php-ext-install exif fileinfo gd intl mbstring pdo_mysql opcache sockets zip

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer \
    && chmod +x /usr/bin/composer

RUN echo "date.timezone = Europe/Stockholm" >> /usr/local/etc/php/php.ini \
    && echo "opcache.enable = 1" >> /usr/local/etc/php/php.ini \
    && echo "opcache.enable_cli = 1" >> /usr/local/etc/php/php.ini \
    && echo "opcache.memory_consumption = 256" >> /usr/local/etc/php/php.ini \
    && echo "opcache.max_accelerated_files = 100000" >> /usr/local/etc/php/php.ini \
    && echo "opcache.interned_strings_buffer = 16" >> /usr/local/etc/php/php.ini \
    && echo "realpath_cache_size = 50M" >> /usr/local/etc/php/php.ini \
    && echo "apc.entries_hint = 256000" >> /usr/local/etc/php/php.ini \
    && echo "apc.shm_size = 512M" >> /usr/local/etc/php/php.ini \
    && echo "apc.enabled = 1" >> /usr/local/etc/php/php.ini \
    && echo "apc.enable_cli = 1" >> /usr/local/etc/php/php.ini \
    && echo /usr/local/etc/php/php.ini

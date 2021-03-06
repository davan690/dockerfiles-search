FROM php:7.4-fpm-alpine

WORKDIR /app

RUN apk update \
 && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    icu-dev libxml2-dev libzip-dev zlib-dev \
    imagemagick-dev freetype-dev libjpeg-turbo-dev libpng-dev libwebp-dev \
 && apk add --no-cache git icu imagemagick libintl libzip supervisor libpng zlib \
 && pecl install imagick redis xdebug \
 && docker-php-ext-configure gd \
       --with-freetype \
       --with-jpeg \
       --with-webp \
 && docker-php-ext-enable imagick redis xdebug \
 && docker-php-ext-install -j$(nproc) bcmath intl pcntl mysqli pdo_mysql soap zip gd sockets \
 && docker-php-source delete \
 && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
 && composer global require hirak/prestissimo \
 && apk del -f .build-deps \
 && rm  -rf /tmp/* /var/cache/apk/*

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

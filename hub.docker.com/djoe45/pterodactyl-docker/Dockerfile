FROM alpine:edge

MAINTAINER Cameron Carney <ccarney16@live.com>

ARG VERSION

ENV STARTUP_TIMEOUT=5 \
    PANEL_VERSION=${VERSION}

WORKDIR /var/www/html

RUN \
 apk --update add tar curl gettext nginx php7 php7 php7-bcmath php7-common php7-dom php7-fileinfo \
 php7-fpm php7-gd php7-memcached php7-mbstring php7-openssl php7-pdo php7-phar php7-json \
 php7-pdo_mysql php7-session php7-simplexml php7-tokenizer php7-ctype php7-zlib php7-zip tini \
 && mkdir -p /var/www/html /run/nginx
 
RUN \
 curl -Lo panel.tar.gz https://github.com/Pterodactyl/Panel/archive/v0.7.13.tar.gz \
 && tar --strip-components=1 -xzvf panel.tar.gz \
 && rm panel.tar.gz \
 && find storage -type d > .storage.tmpl \
 && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
 && cp .env.example .env \
 && composer install --ansi --no-dev \
 && chown -R nginx:nginx . && chmod -R 777 storage/* bootstrap/cache

COPY ./ /

VOLUME [ "/data" ]

# Expose HTTP and HTTPS ports
EXPOSE 80 443

ENTRYPOINT [ "/sbin/tini", "--", "/entrypoint.sh" ]

RUN ["chmod", "+x", "/entrypoint.sh"]

CMD [ "p:start" ]

FROM httpd:alpine

MAINTAINER tim <tim@soxbox.co>

# dependencies required for running "phpize"
# these get automatically installed and removed by "docker-php-ext-*" (unless they're already installed)
ENV PHPIZE_DEPS \
		autoconf \
		dpkg-dev dpkg \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pkgconf \
		re2c

# persistent / runtime deps
RUN apk add --no-cache --virtual .persistent-deps \
		ca-certificates \
		curl \
		tar \
		xz \
# https://github.com/docker-library/php/issues/494
		openssl


ENV PHP_INI_DIR /usr/local/etc/php
RUN set -eux; \
	mkdir -p "$PHP_INI_DIR/conf.d";

ENV PHP_EXTRA_BUILD_DEPS apache2-dev
# ENV PHP_EXTRA_CONFIGURE_ARGS --with-apxs2 --disable-cgi

##<autogenerated>##
##</autogenerated>##

# Apply stack smash protection to functions using local buffers and alloca()
# Make PHP's main executable position-independent (improves ASLR security mechanism, and has no performance impact on x86_64)
# Enable optimization (-O2)
# Enable linker optimization (this sorts the hash buckets to improve cache locality, and is non-default)
# Adds GNU HASH segments to generated executables (this is used if present, and is much faster than sysv hash; in this configuration, sysv hash is also generated)
# https://github.com/docker-library/php/issues/272
ENV PHP_CFLAGS="-fstack-protector-strong -fpic -fpie -O2"
ENV PHP_CPPFLAGS="$PHP_CFLAGS"
ENV PHP_LDFLAGS="-Wl,-O1 -Wl,--hash-style=both -pie"
ENV GPG_KEYS 1A4E8B7277C42E53DBA9C7B9BCAA30EA9C0D5763 6E4F6AB321FDC07F2C332E3AC2BF0BC433CFC8B3

ENV PHP_VERSION 7.0.33
ENV PHP_URL="https://secure.php.net/get/php-7.0.33.tar.xz/from/this/mirror" PHP_ASC_URL="https://secure.php.net/get/php-7.0.33.tar.xz.asc/from/this/mirror"
ENV PHP_SHA256="ab8c5be6e32b1f8d032909dedaaaa4bbb1a209e519abb01a52ce3914f9a13d96" PHP_MD5=""

RUN set -eux; \
	\
	apk add --no-cache --virtual .fetch-deps gnupg; \
	\
	mkdir -p /usr/src; \
	cd /usr/src; \
	\
	curl -fsSL -o php.tar.xz "$PHP_URL"; \
	\
	if [ -n "$PHP_SHA256" ]; then \
		echo "$PHP_SHA256 *php.tar.xz" | sha256sum -c -; \
	fi; \
	if [ -n "$PHP_MD5" ]; then \
		echo "$PHP_MD5 *php.tar.xz" | md5sum -c -; \
	fi; \
	\
	if [ -n "$PHP_ASC_URL" ]; then \
		curl -fsSL -o php.tar.xz.asc "$PHP_ASC_URL"; \
		export GNUPGHOME="$(mktemp -d)"; \
		for key in $GPG_KEYS; do \
			gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
		done; \
		gpg --batch --verify php.tar.xz.asc php.tar.xz; \
		gpgconf --kill all; \
		rm -rf "$GNUPGHOME"; \
	fi; \
	\
	apk del --no-network .fetch-deps

COPY docker-php-source /usr/local/bin/

RUN set -eux; \
	apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
		coreutils \
		curl-dev \
		libedit-dev \
		libxml2-dev \
		openssl-dev \
		sqlite-dev \
		${PHP_EXTRA_BUILD_DEPS:-} \
	; \
	\
	export CFLAGS="$PHP_CFLAGS" \
		CPPFLAGS="$PHP_CPPFLAGS" \
		LDFLAGS="$PHP_LDFLAGS" \
	; \
	docker-php-source extract; \
	cd /usr/src/php; \
	gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
	./configure \
		--build="$gnuArch" \
		--with-config-file-path="$PHP_INI_DIR" \
		--with-config-file-scan-dir="$PHP_INI_DIR/conf.d" \
		\
# make sure invalid --configure-flags are fatal errors intead of just warnings
		--enable-option-checking=fatal \
		\
# https://github.com/docker-library/php/issues/439
		--with-mhash \
		\
# --enable-ftp is included here because ftp_ssl_connect() needs ftp to be compiled statically (see https://github.com/docker-library/php/issues/236)
		--enable-ftp \
# --enable-mbstring is included here because otherwise there's no way to get pecl to use it properly (see https://github.com/docker-library/php/issues/195)
		--enable-mbstring \
# --enable-mysqlnd is included here because it's harder to compile after the fact than extensions are (since it's a plugin for several extensions, not an extension in itself)
		--enable-mysqlnd \
		\
		--with-curl \
		--with-libedit \
		--with-openssl \
		--with-zlib \
		--with-apxs2 \
		--disable-cgi \
		\
# bundled pcre does not support JIT on s390x
# https://manpages.debian.org/stretch/libpcre3-dev/pcrejit.3.en.html#AVAILABILITY_OF_JIT_SUPPORT
		$(test "$gnuArch" = 's390x-linux-gnu' && echo '--without-pcre-jit') \
		\
		${PHP_EXTRA_CONFIGURE_ARGS:-} \
	; \
	make -j "$(nproc)"; \
	find -type f -name '*.a' -delete; \
	make install; \
	find /usr/local/bin /usr/local/sbin -type f -perm +0111 -exec strip --strip-all '{}' + || true; \
	make clean; \
	\
# https://github.com/docker-library/php/issues/692 (copy default example "php.ini" files somewhere easily discoverable)
	cp -v php.ini-* "$PHP_INI_DIR/"; \
	\
	cd /; \
	docker-php-source delete; \
	\
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --no-cache $runDeps; \
	\
	apk del --no-network .build-deps; \
	\
# update pecl channel definitions https://github.com/docker-library/php/issues/443
	pecl update-channels; \
	rm -rf /tmp/pear ~/.pearrc; \
# smoke test
	php --version

COPY docker-php-ext-* docker-php-entrypoint /usr/local/bin/

# ADD conf/php.ini /usr/local/etc/php/php.ini
# ADD conf/www.conf /usr/local/etc/php-fpm.d/www.conf

ENV IMAGICK_VERSION 3.4.2
# RUN apk update && apk upgrade
RUN apk add --update git make gcc g++ imagemagick-dev \
  libc-dev \
  autoconf \
  freetype-dev \
  libjpeg-turbo-dev \
  libpng-dev \
  libmcrypt-dev \
#   libpcre32 \
  bzip2 \
  libbz2 \
  bzip2-dev \
  libmemcached-dev \
  cyrus-sasl-dev \
  binutils \
  && pecl install imagick-$IMAGICK_VERSION \
  && rm -rf /var/cache/apk/*

RUN apk update && apk add ca-certificates && \
  apk add tzdata && \
  ln -sf /usr/share/zoneinfo/US/Arizona /etc/localtime && \
  echo "Us/Arizona" > /etc/timezone

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install gd \
  && docker-php-ext-install mysqli \
  && docker-php-ext-install bz2 \
  && docker-php-ext-install zip \
  && docker-php-ext-install pdo \
  && docker-php-ext-install pdo_mysql \
  && docker-php-ext-install opcache \
  && echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini \
  && echo "extension=bcmath.so" > /usr/local/etc/php/conf.d/bcmath.ini \
  && echo "zend_extension=xdebug.so" >> /usr/local/etc/php/conf.d/xdebug.ini 	
#   && echo "extension=imagick.so" >> /usr/local/etc/php/conf.d/imagick.ini


ENV PHALCON_VERSION=3.4.2

WORKDIR /usr/src/php/ext/

# Compile Phalcon
RUN set -xe && \
  curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz && \
  tar xzf v${PHALCON_VERSION}.tar.gz && cd cphalcon-${PHALCON_VERSION}/build && sh install && \
  echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini && \
  cd ../.. && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION} 

#Compile XDebug
RUN set -xe && \
  curl -LO https://github.com/xdebug/xdebug/archive/XDEBUG_2_5_0.tar.gz && \
  tar xzf XDEBUG_2_5_0.tar.gz && cd xdebug-XDEBUG_2_5_0 && \
  phpize && ./configure --enable-xdebug && make && make install && \
  cd ../ && rm -rf xdebug-XDEBUG_2_5_0

RUN docker-php-source extract \
  && cd /usr/src/php/ext/bcmath \
  && phpize && ./configure --with-php-config=/usr/local/bin/php-config && make && make install \
  && make clean \
  && docker-php-source delete

# RUN set -xe && \
# 	curl -LO https://github.com/ImageMagick/ImageMagick/archive/6.9.6-8.tar.gz && \
# 	tar xzf 6.9.6-8.tar.gz && cd ImageMagick-6.9.6-8 && ./configure --with-bzlib=yes --with-fontconfig=yes --with-freetype=yes --with-gslib=yes --with-gvc=yes --with-jpeg=yes --with-jp2=yes --with-png=yes --with-tiff=yes && make clean && make && make install && \
# 	make clean && ldconfig /usr/local/lib

# RUN set -xe && \
# 	curl -LO https://github.com/imagick/imagick/archive/3.4.2.tar.gz && \
# 	tar zxvf  3.4.2.tar.gz && cd imagick-3.4.2 && phpize && ./configure  --with-php-config=/usr/local/bin/php-config --with-imagick=/usr/local/lib && make && make install

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

EXPOSE 80
CMD ["httpd-foreground"]
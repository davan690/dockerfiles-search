FROM lsiobase/alpine:latest
# args
ARG BUILD_DATE
ARG VERSION
ARG BUILD_CORES
ARG MEDIAINF_VER="19.04"
ARG RTORRENT_VER="0.9.6"
ARG LIBTORRENT_VER="0.13.6"
ARG CURL_VER="7.61.1"

# set env
ENV PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
ENV LD_LIBRARY_PATH=/usr/local/lib

# install runtime packages
RUN NB_CORES=${BUILD_CORES-`getconf _NPROCESSORS_CONF`} && \
 apk add --no-cache \
		ca-certificates \
		python2-dev \
		python3-dev \
		libffi-dev \
		procps \
		sox \
		fcgi \
		ffmpeg \
		geoip \
		gzip \
		logrotate \
		nginx \
		dtach \
		tar \
		unrar \
		unzip \
		wget \
		irssi \
		irssi-perl \
		zlib \
		zlib-dev \
		libxml2-dev \
		perl-archive-zip \
		perl-net-ssleay \
		perl-digest-sha1 \
		git \
		libressl \
		binutils \
		php7 \
        php7-cgi \
        php7-fpm \
        php7-json  \
        php7-mbstring \
		php7-sockets \
        php7-pear \
	python3-dev \
        zip && \
# install build packages
 apk add --no-cache --virtual=build-dependencies \
        autoconf \
        automake \
		perl-dev \
        file \
        g++ \
        gcc \
        libtool \
        make \
        ncurses-dev \
		build-base \
		libtool \
		subversion \
		linux-headers \
		curl-dev \
        libressl-dev && \
# install cppunit older packages
 apk add --no-cache  --repository http://dl-3.alpinelinux.org/alpine/v3.6/main/ \
		cppunit=1.13.2-r1  \
		cppunit-dev=1.13.2-r1  && \
# install cfscrape for rutorrent plugin fix
pip3 install cfscrape cloudscraper && \
# compile curl to fix ssl for rtorrent
cd /tmp && \
mkdir curl && \
cd curl && \
wget -qO- https://curl.haxx.se/download/curl-${CURL_VER}.tar.gz | tar xz --strip 1 && \
./configure --with-ssl && make -j ${NB_CORES} && make install && \
ldconfig /usr/bin && ldconfig /usr/lib && \
# install webui
 mkdir -p \
        /usr/share/webapps/rutorrent \
        /defaults/rutorrent-conf && \
 git clone https://github.com/Novik/ruTorrent.git \
        /usr/share/webapps/rutorrent/ && \
 mv /usr/share/webapps/rutorrent/conf/* \
        /defaults/rutorrent-conf/ && \
 rm -rf \
        /defaults/rutorrent-conf/users && \
# install autodl-irssi perl modules
 perl -MCPAN -e 'my $c = "CPAN::HandleConfig"; $c->load(doit => 1, autoconfig => 1); $c->edit(prerequisites_policy => "follow"); $c->edit(build_requires_install_policy => "yes"); $c->commit' && \
 curl -L http://cpanmin.us | perl - App::cpanminus && \
	cpanm HTML::Entities XML::LibXML JSON JSON::XS && \
# compile xmlrpc-c
cd /tmp && \
#svn checkout http://svn.code.sf.net/p/xmlrpc-c/code/stable xmlrpc-c && \
mkdir xmlrpc-c && \
cd /tmp/xmlrpc-c && \
wget -qO- https://sourceforge.net/projects/xmlrpc-c/files/latest/download?source=files | tar xz --strip 1 && \
./configure --with-libwww-ssl --disable-wininet-client --disable-curl-client --disable-libwww-client --disable-abyss-server --disable-cgi-server && make -j ${NB_CORES} && make install && \
# compile libtorrent
cd /tmp && \
mkdir libtorrent && \
cd libtorrent && \
wget -qO- http://rtorrent.net/downloads/libtorrent-${LIBTORRENT_VER}.tar.gz | tar xz --strip 1 && \
./autogen.sh && ./configure && make -j ${NB_CORES} && make install && \
# compile rtorrent
cd /tmp && \
mkdir rtorrent && \
cd rtorrent && \
wget -qO- http://rtorrent.net/downloads/rtorrent-${RTORRENT_VER}.tar.gz | tar xz --strip 1 && \
./autogen.sh && ./configure --with-xmlrpc-c && make -j ${NB_CORES} && make install && \
# compile mediainfo packages
 curl -o \
 /tmp/libmediainfo.tar.gz -L \
        "http://mediaarea.net/download/binary/libmediainfo0/${MEDIAINF_VER}/MediaInfo_DLL_${MEDIAINF_VER}_GNU_FromSource.tar.gz" && \
 curl -o \
 /tmp/mediainfo.tar.gz -L \
        "http://mediaarea.net/download/binary/mediainfo/${MEDIAINF_VER}/MediaInfo_CLI_${MEDIAINF_VER}_GNU_FromSource.tar.gz" && \
 mkdir -p \
        /tmp/libmediainfo \
        /tmp/mediainfo && \
 tar xf /tmp/libmediainfo.tar.gz -C \
        /tmp/libmediainfo --strip-components=1 && \
 tar xf /tmp/mediainfo.tar.gz -C \
        /tmp/mediainfo --strip-components=1 && \
 cd /tmp/libmediainfo && \
        ./SO_Compile.sh && \
 cd /tmp/libmediainfo/ZenLib/Project/GNU/Library && \
        make install && \
 cd /tmp/libmediainfo/MediaInfoLib/Project/GNU/Library && \
        make install && \
 cd /tmp/mediainfo && \
        ./CLI_Compile.sh && \
 cd /tmp/mediainfo/MediaInfo/Project/GNU/CLI && \
        make install && \
 echo "**** fix logrotate ****" && \
 sed -i "s#/var/log/messages {}.*# #g" /etc/logrotate.conf && \
 echo "**** cleanup ****" && \
 rm -rf \
	/etc/nginx/conf.d/default.conf \
/tmp/*
# add local files
COPY root/ /
# ports and volumes
EXPOSE 443 51500
VOLUME /config /downloads /postprocess

FROM marcellodesales/gradle-java
MAINTAINER AS Ideas <hello@asideas.de>

# Setup Firefox and the fonts
RUN \
apt-get update && \
apt-get install -y \
firefox \
ca-certificates \
imagemagick \
upx \
xfonts-100dpi \
xfonts-75dpi \
xfonts-scalable \
xfonts-cyrillic \
xvfb \
libxtst6 \
cabextract \
dbus-x11 \
openssl \
--no-install-recommends && \
apt-get clean autoclean && \
rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
upx --best /usr/lib/firefox/firefox 

# Solvin the link of the latest library https://github.com/marcellodesales/gradle-java-docker/blob/master/Dockerfile#L25
RUN \
mkdir -p /usr/lib/mozilla/plugins && \
ln -s /opt/java/jre/lib/amd64/libnpjp2.so /usr/lib/mozilla/plugins

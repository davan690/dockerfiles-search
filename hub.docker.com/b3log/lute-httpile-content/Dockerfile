# SFTPGo build stage
FROM golang:stretch as sftpgo
LABEL maintainer="nicola.murino@gmail.com"

RUN go get -d github.com/drakkan/sftpgo
WORKDIR /go/src/github.com/drakkan/sftpgo

ARG build_commit="8e604f888a3a3c324500ba261cd1789ee8d80f0d"

# uncomment the next line to get the latest stable version instead of the latest git
# RUN git checkout `git rev-list --tags --max-count=1`
RUN go build -i -ldflags "-s -w -X github.com/drakkan/sftpgo/utils.commit=`git describe --always --dirty` -X github.com/drakkan/sftpgo/utils.date=`date -u +%FT%TZ`" -o sftpgo

#
# Main build stage
#
FROM ubuntu:bionic

# Set time zone
ENV TZ="UTC"
RUN echo $TZ > /etc/timezone

# Run base build process
COPY . /bd_build

COPY --from=sftpgo /go/src/github.com/drakkan/sftpgo/sftpgo /usr/local/sbin/sftpgo

RUN chmod a+x /bd_build/*.sh \
    && /bd_build/prepare.sh \
    && /bd_build/add_user.sh \
    && /bd_build/setup.sh \
    && /bd_build/cleanup.sh \
    && rm -rf /bd_build

#
# START Operations as `azuracast` user
#
USER azuracast

# Add global Composer deps
RUN composer create-project azuracast/azuracast /var/azuracast/new ^0.9.8 --no-dev \
    && mv /var/azuracast/new/vendor /var/azuracast/www \
    && rm -rf /var/azuracast/new \
    && touch /var/azuracast/.docker

# Clone repo and set up AzuraCast repo
WORKDIR /var/azuracast/www
VOLUME ["/var/azuracast/www", "/var/azuracast/backups", "/etc/letsencrypt", "/var/azuracast/sftpgo/persist"]

#
# END Operations as `azuracast` user
#
USER root

EXPOSE 80 443 2022

# Nginx Proxy environment variables.
ENV VIRTUAL_HOST="azuracast.local" \
    HTTPS_METHOD="noredirect"

# Sensible default environment variables.
ENV APPLICATION_ENV="production" \
    MYSQL_HOST="mariadb" \
    MYSQL_PORT=3306 \
    MYSQL_USER="azuracast" \
    MYSQL_PASSWORD="azur4c457" \
    MYSQL_DATABASE="azuracast" \ 
    PREFER_RELEASE_BUILDS="false" \
    COMPOSER_PLUGIN_MODE="false" \
    ADDITIONAL_MEDIA_SYNC_WORKER_COUNT=0

# Entrypoint and default command
ENTRYPOINT ["dockerize",\
    "-wait","tcp://mariadb:3306",\
    "-wait","tcp://influxdb:8086",\
    "-timeout","40s"]
CMD ["/usr/local/bin/my_init"]

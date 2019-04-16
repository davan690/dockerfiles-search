FROM ypcs/debian:buster

ARG APT_PROXY

RUN /usr/lib/docker-helpers/apt-setup && \
    /usr/lib/docker-helpers/apt-upgrade && \
    apt-get --assume-yes install \
        knot && \
    /usr/lib/docker-helpers/apt-cleanup

COPY entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

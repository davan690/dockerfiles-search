ARG arch=armhf
FROM multiarch/alpine:${arch}-v3.9
COPY qemu-arm-static /usr/bin/
MAINTAINER Spencer Julian <hellothere@spencerjulian.com>

RUN apk update \
 && apk upgrade \
 && apk add --update curl wget bash ruby ruby-bundler python3 python3-dev py3-pip dumb-init musl linux-headers build-base libffi-dev openssl-dev ruby-rdoc ruby-irb\
 && rm -rf /var/cache/apk/* \
 && mkdir /geemusic

COPY . /geemusic
WORKDIR /geemusic

RUN pip3 install -U 'pip<10' && pip3 install -r requirements.txt \
 && gem install foreman

EXPOSE 5000

# Make sure to run with the GOOGLE_EMAIL, GOOGLE_PASSWORD, and APP_URL environment vars!
ENTRYPOINT ["/usr/bin/dumb-init"]
CMD ["foreman", "start"]

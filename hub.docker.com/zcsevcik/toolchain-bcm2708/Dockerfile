FROM frolvlad/alpine-glibc:latest
LABEL maintainer "zcsevcik@gmail.com"

RUN apk --update --no-cache add --virtual build-dependencies \
    git ca-certificates && \

    git clone --depth 1 https://github.com/raspberrypi/tools.git /tmp/raspberrypi-tools && \
    mv /tmp/raspberrypi-tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/ /opt/ && \
    rm -fr /tmp/raspberrypi-tools/ && \

    apk del build-dependencies

ENV PATH=/opt/arm-rpi-4.9.3-linux-gnueabihf/bin:$PATH

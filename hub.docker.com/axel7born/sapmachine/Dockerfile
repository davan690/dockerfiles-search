
FROM ubuntu:16.04

MAINTAINER Axel Siebenborn <axel.siebenborn@sap.com>

RUN rm -rf /var/lib/apt/lists/* && apt-get clean && apt-get update \
    && apt-get install -y --no-install-recommends curl ca-certificates \
    && rm -rf /var/lib/apt/lists/*

ENV JAVA_VERSION jdk-10+32

RUN set -eux;\
    cd /tmp;\
    curl -Lso sapmachine_linux-x64-jdk-10.32.tar.gz https://github.com/SAP/SapMachine/releases/download/jdk-10%2B32/sapmachine_linux-x64-jdk-10.32.tar.gz;\
    curl -Ls https://github.com/SAP/SapMachine/releases/download/jdk-10%2B32/sapmachine_linux-x64-jdk-10.32.sha256.txt | sha256sum -c -;\
    mkdir -p /opt/java/sapmachine;\
    cd /opt/java/sapmachine;\
    tar -xf /tmp/sapmachine_linux-x64-jdk-10.32.tar.gz;\
    rm -f /tmp/sapmachine_linux-x64-jdk-10.32.tar.gz;

ENV PATH=/opt/java/sapmachine/jdk/bin:$PATH


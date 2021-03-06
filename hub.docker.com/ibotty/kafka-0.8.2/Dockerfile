FROM jboss/base-jdk:7

MAINTAINER http://openshift.io

USER root

ENV KAFKA_VERSION 0.8.2.1
ENV SCALA_VERSION 2.10
ENV KAFKA_URL http://www.us.apache.org/dist/kafka/${KAFKA_VERSION}/kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz 
EXPOSE 2181 2888 3888

RUN  curl -L ${KAFKA_URL} \
       | tar xz -C /opt \
    && yum install -y gettext $$ yum clean all \
    && mv /opt/kafka_${SCALA_VERSION}-${KAFKA_VERSION} /opt/kafka \
    && mkdir -p /opt/kafka/logs

WORKDIR /opt/kafka

# Fix the permissions when running in OpenShift
# has to be _before_ the volume declaration
RUN chmod -R a+rwx /opt/kafka/config /opt/kafka/logs

VOLUME ["/opt/kafka/config", "/opt/kafka/logs"]

COPY config-and-run.sh ./bin/
COPY server-template.properties ./config/

CMD ["/opt/kafka/bin/config-and-run.sh"]

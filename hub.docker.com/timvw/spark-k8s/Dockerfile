FROM registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift:latest

ENV SPARK_VERSION 2.4.4
ENV HADOOP_VERSION 2.7
ENV SPARK_PACKAGE spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}
ENV SPARK_PACKAGE_URL=http://apache.belnet.be/spark/spark-${SPARK_VERSION}/${SPARK_PACKAGE}.tgz

ENV SPARK_HOME /opt/spark-${SPARK_VERSION}
ENV PATH $PATH:${SPARK_HOME}/bin

USER root

RUN mkdir -p ${SPARK_HOME} \
    && curl -sL --retry 3 "${SPARK_PACKAGE_URL}" | gunzip | tar x -C /opt/ \
    && mv /opt/$SPARK_PACKAGE/* $SPARK_HOME \
    && rm -rf /opt/$SPARK_PACKAGE \
    && chmod -R u+x ${SPARK_HOME}/bin \
    && chgrp -R 0 ${SPARK_HOME} \
    && chmod -R g=u ${SPARK_HOME} /etc/passwd

USER 10001

WORKDIR $SPARK_HOME

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "bin/spark-class", "org.apache.spark.deploy.master.Master" ]


FROM openjdk:8-jdk-slim

ARG JIRA_VERSION=7.1.9
ENV JIRA_VERSION=$JIRA_VERSION
ARG JIRA_HOST_PORT=8080
ENV JIRA_HOST_PORT=$JIRA_HOST_PORT
ARG JIRA_DOCKER_PORT=8080
ENV JIRA_DOCKER_PORT=$JIRA_DOCKER_PORT
ENV JIRA_BINARY=atlassian-jira-software-${JIRA_VERSION}-x64.bin

RUN apt-get update -y; apt-get install wget -y
RUN cd /tmp; wget https://www.atlassian.com/software/jira/downloads/binary/${JIRA_BINARY}
RUN chmod +x /tmp/${JIRA_BINARY}

ADD ./files/response.varfile /tmp

RUN /tmp/${JIRA_BINARY} -q -varfile /tmp/response.varfile

RUN chown -R jira /opt/atlassian/jira ${JIRA_HOME}

CMD ["/opt/atlassian/jira/bin/start-jira.sh", "run"]




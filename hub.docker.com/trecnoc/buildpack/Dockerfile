FROM openjdk:8-jdk-stretch

# Mkdir fixes a debian bug: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=863199
RUN mkdir -p /usr/share/man/man1 && \
    apt-get update && \
    apt-get install -y curl wget gnupg2 apt-transport-https sudo

# Add cf, and node repositories
# Install cf, node, flyway and maven
RUN wget -q -O - https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | apt-key add - && \
    echo "deb https://packages.cloudfoundry.org/debian stable main" | tee /etc/apt/sources.list.d/cloudfoundry-cli.list && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    wget -qO- https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/5.2.4/flyway-commandline-5.2.4-linux-x64.tar.gz | tar xvz && sudo ln -s `pwd`/flyway-5.2.4/flyway /usr/local/bin && \
    chmod -R a+rx-w flyway-5.2.4/ && \
    apt-get update && \
    apt-get install -y cf-cli nodejs maven git && \
    rm -rf /var/lib/apt/lists/* && \
    useradd --create-home --shell /bin/bash buildrunner

USER buildrunner
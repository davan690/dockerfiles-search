FROM openjdk:8

LABEL maintainer="Yohan Alard <y.alard@csb.nc>"

RUN apt-get update
RUN apt-get install -y curl git tmux htop maven sudo

# Install Node - allows for scanning of Typescript
RUN curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
RUN sudo apt-get install -y nodejs build-essential

WORKDIR /usr/src

RUN curl --insecure -o ./sonarscanner.zip -L https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.3.0.1492-linux.zip && \
	unzip sonarscanner.zip && \
	rm sonarscanner.zip && \
	mv sonar-scanner-3.3.0.1492-linux /usr/lib/sonar-scanner && \
	ln -s /usr/lib/sonar-scanner/bin/sonar-scanner /usr/local/bin/sonar-scanner

ENV SONAR_RUNNER_HOME=/usr/lib/sonar-scanner

COPY sonar-runner.properties /usr/lib/sonar-scanner/conf/sonar-scanner.properties

RUN useradd --system --gid root --uid 1000 --shell /bin/bash --create-home bob_the_builder && usermod -aG sudo bob_the_builder



# Use bash if you want to run the environment from inside the shell, otherwise use the command that actually runs the underlying stuff
#CMD /bin/bash
CMD sonar-scanner -Dsonar.projectBaseDir=/usr/src

FROM jenkinsci/jenkins:lts
USER root
RUN apt-get update
RUN apt-get install -y maven
RUN apt-get install -y wget
RUN wget https://github.com/openshift/origin/releases/download/v1.5.0/openshift-origin-client-tools-v1.5.0-031cbe4-linux-64bit.tar.gz
RUN mv openshift-origin-client-tools-v1.5.0-031cbe4-linux-64bit.tar.gz /opt/
RUN tar -zxvf /opt/openshift-origin-client-tools-v1.5.0-031cbe4-linux-64bit.tar.gz -C /opt/
RUN rm /opt/openshift-origin-client-tools-v1.5.0-031cbe4-linux-64bit.tar.gz
RUN echo 'export OC_CLIENT=/opt/openshift-origin-client-tools-v1.5.0-031cbe4-linux-64bit\nexport PATH=$OC_CLIENT:$PATH\n' >> /etc/bash.bashrc
RUN rm -rf /var/lib/apt/lists/*
USER jenkins

FROM golang:1.11-stretch

# Install unzip and Python build tools to be able to install the AWS CLI tools.
RUN apt-get update -y && \
    apt-get install -y zip unzip python-dev curl make git openssh-client

# Setup SSH host verification for github.com (required for go get).
RUN mkdir -p ~/.ssh/ && \
    ssh-keygen -F github.com || ssh-keyscan github.com >>~/.ssh/known_hosts

# Install Node
RUN curl -sL https://deb.nodesource.com/setup_11.x -o setup_11.x && \
chmod +x setup_11.x && \
./setup_11.x && \
rm setup_11.x && \
apt-get install nodejs

# Install AWS CLI tools.
RUN curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip" && \
    unzip awscli-bundle.zip && \
    ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws

# Install docker
RUN VER="18.06.1-ce" && curl -L -o /tmp/docker-$VER.tgz https://download.docker.com/linux/static/stable/x86_64/docker-$VER.tgz && tar -xz -C /tmp -f /tmp/docker-$VER.tgz && mv /tmp/docker/* /usr/bin

# Install Serverless Framework.
RUN npm install -g serverless@1.36.3

ENV TZ=Europe/London
ENV NODE_ENV=dev
ENV AWS_DEFAULT_REGION=eu-west-2

# Install Go "deployer" Well deployment tool
RUN go get github.com/welldigital/deployer

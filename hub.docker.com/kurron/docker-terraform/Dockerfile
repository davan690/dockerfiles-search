FROM ubuntu:14.04 

MAINTAINER Ron Kurr <kurr@kurron.org>

LABEL org.kurron.ide.name="Terraform" org.kurron.ide.version=0.6.16

ADD https://releases.hashicorp.com/terraform/0.6.16/terraform_0.6.16_linux_amd64.zip /tmp/ide.zip

RUN apt-get update && \
    apt-get install -y unzip ca-certificates git && \
    unzip /tmp/ide.zip -d /usr/local/bin && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/*

RUN chmod 0555 /usr/local/bin/*

# the user of this image is expected to mount his actual home directory to this one
VOLUME ["/home/developer"]
VOLUME ["/pwd"]

# Set the AWS environment variables
ENV AWS_ACCESS_KEY_ID OVERRIDE ME
ENV AWS_SECRET_ACCESS_KEY OVERRIDE_ME
ENV AWS_REGION us-west-2

ENV HOME /home/developer
WORKDIR /pwd
ENTRYPOINT ["/usr/local/bin/terraform"]
CMD ["--version"]

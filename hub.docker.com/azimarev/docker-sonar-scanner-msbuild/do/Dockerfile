FROM ubuntu-upstart:14.04
MAINTAINER Kamil Trzciński <ayufan@ayufan.eu>

# Install required dependencies
RUN apt-get update && \
	apt-get install -y apt-transport-https locales git make \
	curl software-properties-common \
	nginx dnsutils aufs-tools \
	dpkg-dev openssh-server man-db \
	apache2-utils
RUN chmod ugo+s /usr/bin/sudo

# Configure environment
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

# Install docker
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9 && \
	echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list && \
	echo "#!/bin/sh\nexit 101" > /usr/sbin/policy-rc.d && \
	chmod +x /usr/sbin/policy-rc.d && \
	apt-get update && \
	apt-get install -y lxc-docker && \
	rm /usr/sbin/policy-rc.d

# Configure volumes
VOLUME /var/lib/docker

FROM ubuntu:bionic
LABEL maintainer "Daniel R. Kerr <daniel.r.kerr@gmail.com>"

ENV DEBIAN_FRONTEND noninteractive
ENV TERM xterm

# install core dependencies
#---------------------------------------
RUN apt-get update -y \
 && apt-get install -qq -y libev-dev libreadline-dev libtk-img libtool \
 && apt-get install -qq -y python3 python3-dev python3-pip python3-setuptools \
 && apt-get install -qq -y autoconf automake gawk gcc git pkg-config tk \
 && apt-get install -qq -y bridge-utils ebtables ethtool iproute2 \
 && apt-get clean \
 && rm -rf /tmp/* /var/lib/apt/lists/* /var/tmp/*

# install ospf mdf
#---------------------------------------
RUN git clone https://github.com/USNavalResearchLaboratory/ospf-mdr.git /opt/ospf-mdr \
 && cd /opt/ospf-mdr \
 && ./bootstrap.sh \
 && ./configure \
    --disable-doc \
    --enable-group=root \
    --enable-user=root \
    --enable-vtysh \
    --localstatedir=/var/run/quagga \
    --sysconfdir=/usr/local/etc/quagga \
    --with-cflags=-ggdb \
 && make \
 && make install \
 && cd \
 && rm -rf /opt/ospf-mdr

# install core
#---------------------------------------
RUN pip3 install fabric grpcio-tools lxml netaddr

RUN git clone -b develop https://github.com/coreemu/core.git /opt/core \
 && cd /opt/core \
 && ./bootstrap.sh \
 && PYTHON=/usr/bin/python3 ./configure \
 && make \
 && make install \
 && cd \
 && rm -rf /opt/core

ENV PYTHONPATH "${PYTHONPATH}:/usr/local/lib/python3.6/site-packages"

# configure core
#---------------------------------------
COPY icons /usr/share/core/icons/cisco

RUN apt-get update -y \
 && apt-get install -qq -y bash curl screen wget xvfb \
 && apt-get install -qq -y apache2 iptables isc-dhcp-server mgen vsftpd \
 && apt-get install -qq -y iputils-ping moreutils net-tools scamper tcpdump traceroute tshark \
 && apt-get clean \
 && rm -rf /tmp/* /var/lib/apt/lists/* /var/tmp/*

# install emanes
#---------------------------------------
#RUN wget -O /opt/emane.tgz https://adjacentlink.com/downloads/emane/emane-1.2.5-release-1.ubuntu-18_04.amd64.tar.gz \
# && cd /opt \
# && tar xzf /opt/emane.tgz \
# && cd /opt/emane-1.2.5-release-1/debs/ubuntu-18_04/amd64 \
# && dpkg -i *.deb \
# && apt-get install -f \
# && cd /root \
# && rm -rf /opt/emane.tgz /opt/emane-1.2.5-release-1

# install and configure ssh
#---------------------------------------
RUN apt-get update -y \
 && apt-get install -qq -y openssh-server \
 && apt-get clean \
 && rm -rf /tmp/* /var/lib/apt/lists/* /var/tmp/*

RUN mkdir /var/run/sshd \
 && mkdir /root/.ssh \
 && chmod 700 /root/.ssh \
 && chown root:root /root/.ssh \
 && touch /root/.ssh/authorized_keys \
 && chmod 600 /root/.ssh/authorized_keys \
 && chown root:root /root/.ssh/authorized_keys \
 && echo "\nX11UseLocalhost no\n" >> /etc/ssh/sshd_config

# install and configure supervisord
#---------------------------------------
RUN apt-get update -y \
 && apt-get install -qq -y supervisor \
 && apt-get clean \
 && rm -rf /tmp/* /var/lib/apt/lists/* /var/tmp/*

COPY supervisord.conf /etc/supervisor/conf.d/core.conf

# startup configuration
#---------------------------------------
EXPOSE 22
EXPOSE 50051

WORKDIR /root
CMD ["/usr/bin/supervisord", "--nodaemon"]

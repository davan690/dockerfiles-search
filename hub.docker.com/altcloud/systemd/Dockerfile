FROM altcloud/base:p9

MAINTAINER alt-cloud

RUN apt-get update && \
    apt-get install -y systemd; \
    rm -f /var/cache/apt/archives/*.rpm \
          /var/cache/apt/*.bin \
          /var/lib/apt/lists/*.*

RUN ( \
        cd /lib/systemd/system/sysinit.target.wants/; \
        for i in *; do \
            [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; \
        done \
    ); \
    rm -f /lib/systemd/system/multi-user.target.wants/*;\
    rm -f /etc/systemd/system/*.wants/*;\
    rm -f /lib/systemd/system/local-fs.target.wants/*; \
    rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
    rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
    rm -f /lib/systemd/system/basic.target.wants/*;

CMD ["/sbin/init"]

FROM jpuhlman/koji-base-clear:latest 
RUN mkdir -p /usr/share/koji-docker
COPY  \
     install-scripts/deploy-koji-builder.sh \
     install-scripts/deploy-koji-docker.sh \
     install-scripts/gencert.sh \
     install-scripts/globals.sh \
     install-scripts/parameters.sh \
     install-scripts/deploy-mash.sh \
     install-scripts/mash.sh\
     install-scripts/watch-*.sh \
     install-scripts/add-hosts.sh \
     install-scripts/user-add.sh \
     configs/app.list \
     configs/user.list \
     /usr/share/koji-docker/
RUN mkdir -p /usr/sbin/
COPY install-scripts/hostenv.sh /usr/sbin/
RUN chmod 755 /usr/bin/hostenv.sh
RUN chmod 755 /usr/share/koji-docker/*.sh
RUN mkdir -p /etc/mock
COPY configs/mock/* /etc/mock/
RUN mkdir -p /etc/systemd/system/
COPY container-services/koji-setup.service \
     container-services/watch*.service \
     /etc/systemd/system/
RUN systemctl enable koji-setup
RUN systemctl disable swupd-update
RUN mkdir -p /etc/sudoers.d/
RUN echo "kojiadmin  ALL=NOPASSWD: /usr/bin/hostenv.sh" | tee -a /etc/sudoers.d/visudo
RUN echo "root:$(echo 'password' | openssl passwd -1 -stdin):18099::::::" >> /etc/shadow
RUN sed -i /usr/lib/systemd/system/swupd-update.service -e "s,/usr/bin/swupd,/bin/echo,"
RUN sed -i  /usr/lib/python2.7/site-packages/koji/daemon.py -e "s,maxload\ =\ 4.0,maxload\ =\ 8.0,"
CMD [ "/sbin/init" ]

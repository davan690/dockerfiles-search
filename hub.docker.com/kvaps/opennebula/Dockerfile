FROM ubuntu:18.04 as builder
ARG VERSION=5.8.5

# Install build dependencies
RUN apt-get update \
 && apt-get install -y pbuilder debhelper ubuntu-dev-tools bash-completion \
      bison default-jdk flex javahelper libxmlrpc3-client-java \
      libxmlrpc3-common-java libxml2-dev ruby scons dh-systemd \
      npm ruby-ronn rename

# Download sources
RUN wget -O - https://github.com/OpenNebula/one/archive/release-$VERSION.tar.gz | tar xzf - \
 && mv one-release-$VERSION opennebula-$VERSION
WORKDIR /opennebula-$VERSION

# Apply patches
RUN curl https://patch-diff.githubusercontent.com/raw/OpenNebula/one/pull/3569.patch | patch -p1

# Make docs
RUN cd share/man \
 && ./build.sh

# Make sunstone
RUN cd src/sunstone/public \
 && ./build.sh -d \
 && export PATH=$PATH:$PWD/node_modules/.bin \
 && ./build.sh \
 && rm -rf node_modules/

# Prepare packages
RUN tar -czf /opennebula-$VERSION.tar.gz /opennebula-$VERSION/
RUN wget -O - https://github.com/OpenNebula/packages/archive/release-$VERSION.tar.gz | tar xzf - \
 && mv packages-release-$VERSION /packages-$VERSION
WORKDIR /packages-$VERSION
RUN sed -i '/^debuild/,$ s/^/#/' ubuntu1804.sh \
 && ./ubuntu1804.sh /opennebula-$VERSION.tar.gz

# Build packages
WORKDIR /root/build-Ubuntu-18.04/opennebula-$VERSION
RUN apt-get install -y $(dpkg-checkbuilddeps 2>&1 | sed 's/.*Unmet build dependencies: //') \
 && echo build_opennebula.tar.gz >> debian/source/include-binaries \
 && echo xmlrpc-c.tar.gz >> debian/source/include-binaries \
 && dpkg-buildpackage -us -uc \
 && mkdir -p /packages \
 && mv /root/build-Ubuntu-18.04/*.deb /packages/


FROM ubuntu:18.04

# Install opennebula-gems
COPY --from=builder packages/opennebula-common_*.deb packages/ruby-opennebula_*_all.deb packages/opennebula-tools_*.deb /packages/
RUN apt-get update \
 && apt-get -y install /packages/*.deb \
 && sed -i 's/apt-get install/apt-get -y install/g' /usr/share/one/install_gems \
 && echo '0\n' | /usr/share/one/install_gems \
 && mkdir -p /var/log/one /var/lock/one \
 && apt-get -y clean \
 && chown oneadmin: /var/log/one /var/lock/one

# Install opennebula
COPY --from=builder packages/opennebula_*.deb /packages/
RUN apt-get -y update \
 && apt-get -y install mysql-client \
 && dpkg -i packages/opennebula_*.deb || true \
 && apt-get -yf install \
 && dpkg -l opennebula \
 && apt-get -y clean

# Logging to /dev/stdout
RUN sed -i 's/\(SYSTEM *= *\)"file"/\1"std"/' /etc/one/oned.conf \
 && sed -i 's/\(system *= *\)"file"/\1"std"/' /etc/one/sched.conf

USER oneadmin
ENTRYPOINT [ "/usr/bin/oned", "-f" ]
#ENTRYPOINT [ "/usr/bin/mm_sched" ]
